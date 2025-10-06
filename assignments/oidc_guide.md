# OpenID Connect client guide

## Goal

Create a web application that allows users to authenticate using their
credentials in Keycloak.

![Sequence diagram of authorization code flow](./oidc-sequence.drawio.svg)

## Project setup

You don't need any client side scripting to solve the assignment.
Actually I would recommend doing without, because it just adds complexity.

### .NET/C

Open a terminal/cmd/powershell

Navigate to the folder you want to contain the project files.
You can use `cd <foldername>` to navigate and `mkdir <foldername>` to create a
new folder.

Then bootstrap the project using:

```sh
dotnet new mvc
```

### Python

For Python I will recommend
[Flask](https://flask.palletsprojects.com/en/3.0.x/), unless you are already
accustomed to a different framework.

### Node/Express

Open a terminal/cmd/powershell

Navigate to the folder you want to contain the project files.
You can use `cd <foldername>` to navigate and `mkdir <foldername>` to create a
new folder.

```sh
npm init --yes
npm install express express-handlebars node-fetch
npm install --save-dev typescript concurrently nodemon @types/express @types/node @types/express-handlebars @types/node-fetch
npx tsc --init
```

Add following to `tsconfig.json`

```json
{
  "outDir": "./dist"
}
```

And include following to `package.json`

```json
{
  "scripts": {
    "build": "npx tsc",
    "start": "node dist/index.js",
    "dev": "concurrently \"npx tsc --watch\" \"nodemon -q dist/index.js\""
  }
}
```

Create a `index.ts` file with something like:

```typescript
import express from "express";
import { engine } from "express-handlebars"; //Sets our app to use the handlebars engine

const app = express();

app.engine("handlebars", engine());
app.set("view engine", "handlebars");
app.set("views", "./views");

app.get("/", (req, res) => {
  res.render("home", { placeholder: "world" });
});

const port = 3000;
app.listen(port, () =>
  console.log(`App listening to port http://localhost:${port}`),
);
```

Create `views/layouts/main.handlebars`

```handlebars
{% raw %}

<html>
  <head>
    <meta charset="utf-8" />
    <title>Example App</title>
  </head>
  <body>

    {{{body}}}

  </body>
</html>
{% endraw %}
```

And `views/home.handlebars`

```handlebars
{% raw %}
<h1>Hello {{placeholder}}</h1>
{% endraw %}
```

## Endpoints

Your application needs the following endpoints.
If you use different paths for your endpoints you need to adjust accordingly.

| Path      | Name     | Purpose                      |
| --------- | -------- | ---------------------------- |
| /         | Home     | Landing page with login link |
| /login    | Login    | Redirects to Keycloak        |
| /callback | Callback | Exchange code for tokens     |

Home and Callback (redirect URI) should be added to the client settings in [Keycloak admin](http://localhost:8080/admin/)

All endpoints in Keycloak can be found at [http://localhost:8080/realms/master/.well-known/openid-configuration](http://localhost:8080/realms/master/.well-known/openid-configuration)

For the rest of the document, keycloak endpoints are referred to by the property name in
openid-configuration document. Also the document itself is referred to as `config`.

Example: `config.authorization_endpoint`

## Login button

Add the following HTML to the view for you Home endpoint:

```html
<a href="/login">Login</a>
```

## Authorization request

The Login endpoint should redirect the browser to a URL similar to the following:

`http://localhost:8080/realms/master/protocol/openid-connect/auth?client_id=client_id&scope=openid email phone address profile&response_type=code&redirect_uri=http://localhost:5138/Home/Callback&prompt=login&state=97tvtZHsHTV4I5parGxBJ-sRF5Lml_JGmb21VXwtoaE&code_challenge_method=plain&code_challenge=es1kPi2mRaxvo4Y3cb8gRFRmYrpJzyO9FelyjMrgy0w`

| Value        | Description                                      |
| ------------ | ------------------------------------------------ |
| clientId     | Client ID for the client you created in Keycloak |
| callback     | Your endpoint that handles callback              |
| state        | Long random string that you store in cache       |
| codeVerifier | Long random string that you store in cache       |

Note _code_challenge/code_verifier_ are part of the PKCE extension to OAuth 2.0

For generating the random strings you should use a Cryptographically secure pseudorandom number generator (CSPRNG).

### C-Sharp (C#)

```csharp
var parameters = new Dictionary<string, string?>
{
    { "client_id", clientId },
    { "scope", "openid email phone address profile" },
    { "response_type", "code" },
    { "redirect_uri", callback },
    { "prompt", "login" },
    { "state", state },
    { "code_challenge_method", "plain" },
    { "code_challenge", codeVerifier }
};
var authorizationUri = QueryHelpers.AddQueryString(config.authorization_endpoint, parameters);
```

### Python

```python
parameters = {
    "client_id": client_id,
    "scope": "openid email phone address profile",
    "response_type": "code",
    "redirect_uri": redirect_uri,
    "prompt": "login",
    "state": state,
    "code_challenge_method": "S256",
    "code_challenge": create_challenge(code_verifier)
}
redirect_url = f"{authorization_endpoint}?{urllib.parse.urlencode(parameters)}"
```

### TypeScript

```typescript
const parameters = {
  client_id: clientId,
  scope: "openid email phone address profile",
  response_type: "code",
  redirect_uri: callback,
  prompt: "login",
  state: state,
  code_challenge_method: "plain",
  code_challenge: codeVerifier,
};

const authorizationUri = `${config.authorization_endpoint}?${new URLSearchParams(parameters)}`;
```

## Caching

You need to `code_verifier` because you need it to verify the callback.
Use `state` as key in your cache.

In a real app you would use something like database or redis for cache.
However here we simplify a bit and just use an in-memory collection instead.

### C-Sharp (C#)

```csharp
private static readonly Dictionary<string, string> _cache = new();
```

Alternatively you can use

```csharp
HttpContext.Session
```

But you will need to add this to `Program.cs` first

```csharp
// Below builder.Services.AddControllersWithViews();

builder.Services.AddSession(options =>
{
    options.Cookie.HttpOnly = true;
    options.Cookie.IsEssential = true;
});

// Below app.UseAuthorization();

app.UseSession();

```

See [documentation](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/app-state?view=aspnetcore-7.0) for more details.

### Python

See [Flask-Caching](https://flask-caching.readthedocs.io/en/latest/index.html)

### TypeScript

```typescript
const cache = new Map<string, string>();
```

Cache needs to be defined somewhere so it persists across requests.

## Authorization response / callback

After the user authenticates we get an authorization response back on the
Callback endpoint.

In the callback you should:

1. Exchange authorization code for access, refresh and ID tokens.
2. Verify/validate ID token
3. Fetch user info resource using the access token.

You can decode and extract the needed values from query parameters as following.

### C-Sharp (C#)

```csharp
public record AuthorizationResponse(string state, string code);

public async Task<IActionResult> Callback(AuthorizationResponse query)
{
    var (state, code) = query;
    // ...
    return View();
}
```

### Python

```python
@app.route('/callback')
def callback():
    state = request.args.get('state')
    code = request.args.get('code')
    # ...
    return ""
```

### TypeScript

```typescript
type AuthorizationResponse = { state: string; code: string };

app.get("/callback", (req, res) => {
  const { state, code } = req.query as AuthorizationResponse;
  // ...
  res.send();
});
```

## Exchange authorization code for tokens

You need to exchange the authorization code for tokens.

### C-Sharp (C#)

```csharp
var parameters = new Dictionary<string, string?>
{
    { "grant_type", "authorization_code" },
    { "code", code },
    { "redirect_uri", redirectUri },
    { "code_verifier", codeVerifier },
    { "client_id", clientId },
    { "client_secret", clientSecret }
};
var response =
    await new HttpClient().PostAsync(config.token_endpoint, new FormUrlEncodedContent(parameters));
var payload = await response.Content.ReadFromJsonAsync<TokenResponse>();
```

TokenResponse can be defined as

```csharp
public class TokenResponse
{
    public string access_token { init; get; }
    public int expires_in { init; get; }
    public string id_token { init; get; }
    public string scope { init; get; }
    public string token_type { init; get; }
    public string refresh_token { init; get; }
}
```

### Python

```python
parameters = {
    "grant_type": "authorization_code",
    "code": code,
    "redirect_uri": redirect_uri,
    "code_verifier": code_verifier,
    "client_id": client_id,
    "client_secret": client_secret
}
qs = urllib.parse.urlencode(parameters)
return requests.post(f"{token_endpoint}?{qs}", data=parameters).json()
```

### TypeScript

```typescript
const parameters = {
  grant_type: "authorization_code",
  code: code,
  redirect_uri: redirectUri,
  code_verifier: codeVerifier,
  client_id: clientId,
  client_secret: clientSecret,
};
const response = await fetch(config.token_endpoint, {
  method: "POST",
  body: new URLSearchParams(parameters),
});
const payload = await response.json();
return payload as TokenResponse;
```

And TokenResponse defined as

```typescript
type TokenResponse = {
  access_token: string;
  expires_in: number;
  id_token: string;
  scope: string;
  token_type: string;
  refresh_token: string;
};
```

## Fetch user info

Now that you have an access token you can use it to fetch user info

### C-Sharp (C#)

```csharp
var http = new HttpClient
{
    DefaultRequestHeaders =
    {
        { "Authorization", "Bearer " + accessToken }
    }
};
var response = await http.GetAsync(config.userinfo_endpoint);
var content = await response.Content.ReadFromJsonAsync<object?>();
```

### Python

```python
headers = {"Authorization": f"Bearer {access_token}"}
content = requests.get(userinfo_endpoint, headers=headers).json()
```

### TypeScript

```typescript
const response = await fetch(config.userinfo_endpoint, {
  headers: {
    Authorization: "Bearer " + accessToken,
  },
});
const content = await response.json();
```

# Securing your solution

There is a couple of things you need to do to secure your implementation.

## Hash the code verifier

For Authorization Request you should replace `code_challenge` with a base64 url
encoded sha256 hash of `code_verifier`.
Also change `code_challenge_method` to be `S256`.

## Verify ID Token

It is very important that you verify authenticity and integrity of the ID token.

For that we first need to fetch the public part of the key that was used to sign
the token.

### C-Sharp (C#)

Use the nuget package `System.IdentityModel.Tokens.Jwt` to verify ID token.

```csharp
var response = await new HttpClient().GetAsync(config.jwks_uri);
var keys = await response.Content.ReadAsStringAsync();
var jwks = JsonWebKeySet.Create(keys);
jwks.SkipUnresolvedJsonWebKeys = false;
```

Then use `JwtSecurityTokenHandler` to validate/verify the token.

### Python

You can use [PyJWT](https://pyjwt.readthedocs.io/en/latest/index.html) to verify
ID Token.

### TypeScript

```typescript
import jwksClient from "jwks-rsa";

var client = jwksClient({
  jwksUri: config.jwks_uri,
});
function getKey(header: JwtHeader, callback: any) {
  client.getSigningKey(header.kid, function (err, key: any) {
    var signingKey = key.publicKey || key.rsaPublicKey;
    callback(null, signingKey);
  });
}
```

Then use `verify` function from `jsonwebtoken` package to validate/verify the token.

## Session

Last step after establishing a valid user identity is to set up a session in your
web application.
So that verified user identity can persist across HTTP requests.

By following this guide you will be using server-side rendering of HTML (no JavaScript frontend).
Therefore, the best way to maintain a session is with a cookie.

What are [session & cookies](https://stackoverflow.com/questions/11142882/what-are-cookies-and-sessions-and-how-do-they-relate-to-each-other)?

### C-Sharp (C#)

Learn about [Session and state management in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/app-state?view=aspnetcore-7.0).

### Python

See [Flask Sessions](https://flask.palletsprojects.com/en/3.0.x/quickstart/#sessions)

### TypeScript

For express, you can use the [session
middleware](https://www.npmjs.com/package/express-session).
