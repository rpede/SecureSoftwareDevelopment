# Dependencies

## Avoid typo-squatting

A common tactic for distributing malicious package is to register a package with a similar name to another popular package, and hope people mistype the package name.
A way to avoid this issue is to check download count before adding a package to
your project.

I therefore recommend either copy-paste package name from the official documentation.
Or search for the package on the repository site for your programming language.

Unfortunately, not all package repository makes this super easy.

**NPM**

<https://www.npmjs.com/>

Just look at "Weekly downloads" on the panel to the right.

**PyPI**

<https://pypi.org/> doesn't show download stats.
Instead, there are several other sites that provide it such as
<https://pypistats.org/>.

**Maven**

I don't know.
Please tell me if you know.

**NuGet**

<https://www.nuget.org/> shows download numbers directly in the search results.

## Scan dependencies

The projects you work on likely have some dependencies, and those dependencies
have other dependencies.
All that quickly adds up.

New vulnerabilities are often discovered in open source packages.
It is important that you are able to tell if a discovered vulnerability affects
your project and take action quickly.

Manually looking at vulnerabilities lists and comparing with dependencies for
all your project is a hassle and unfeasible for most.

Luckily there are tools out there to automate this process.

### Node

This is very simple, since it is build in to the package manager.

**Scan**

```sh
npm audit
```

**Fix**

```sh
npm audit --fix
```

### Python

For Python packages we need to install a tool.
I strongly recommend using a [virtual
environment](https://realpython.com/python-virtual-environments-a-primer/) to
install packages for your Python projects.

Make sure you have the `venv` for activated for your project.

```sh
pip install pip-audit
```

**Scan**

To scan your local environment.

```sh
python -m pip_audit -l
```

To scan `requirements.txt`.

```sh
python -m pip_audit -r requirements.txt
```

**Fix issues**

```sh
python -m pip_audit -r requirements.txt --fix
```

### .NET

**Scan**

```sh
dotnet list package --vulnerable --include-transitive
```

**Fix**

There is no command built-in to just update vulnerable packages.
What you can do instead is to upgrade all outdated dependencies.

```sh
dotnet tool install --global dotnet-outdated-tool
dotnet outdated --upgrade
```

### Java

Unfortunately a vulnerability isn't built-in.
But, you can fairly easily add one as a plugin.

#### Maven

Add to `pom.xml`

```xml
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>11.0.0</version> <!-- Change to latest version -->
    <executions>
        <execution>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

```sh
mvn verify
```

#### Gradle

Add to `build.gradle`

```groovy
plugins {
    id 'org.owasp.dependencycheck' version '11.0.0' // Change to latest version
}
```

```sh
./gradlew dependencyCheckAnalyze
```

## Software Bill of Materials (SBOM)

### npm

```sh
npm sbom --sbom-format cyclonedx
```

### .NET

See [SBOM Tool](https://github.com/microsoft/sbom-tool)

### Python

CycloneDX can be used to.
It is a dependency of pip-audit, but can also be installed alone with:

```sh
pip install cyclonedx-bom
```

It can generate SBOM from `requirements.txt` with:

```sh
python3 -m cyclonedx_py requirements
```

See [CycloneDX Python SBOM Generation Tool](https://github.com/CycloneDX/cyclonedx-python)
