# CI/CD Goat

The "CI/CD Goat" is a deliberately vulnerable Continuous Integration/Continuous
Deployment (CI/CD) environment designed to learn about various security risks
associated with CI/CD systems.

[Learn more](https://www.cidersecurity.io/blog/research/ci-cd-goat/)

It basically bunch of a CTF (capture the flag) challenges.

To solve a challenge you have to find the flag and submit it to the CTFd dashboard.
Flags takes the form of tokens.
So basically a piece of text.

CI/CD Goat is build with a combination open-source tools centered around version
control and build pipelines.
Pre-existing experience with any of the tools not required to solve the challenges.
Though experience will be an advantage.

Prerequisite is experience with git and CI/CD in some form.
Wether GitHub Actions, BitBucket Pipelines or something else.

## Getting started

**Linux & Mac**
```sh
curl -o cicd-goat/docker-compose.yaml --create-dirs https://raw.githubusercontent.com/cider-security-research/cicd-goat/main/docker-compose.yaml
cd cicd-goat && docker compose up -d
```

**Windows (Powershell)**
```powershell
mkdir cicd-goat; cd cicd-goat
curl -o docker-compose.yaml https://raw.githubusercontent.com/cider-security-research/cicd-goat/main/docker-compose.yaml
get-content docker-compose.yaml | %{$_ -replace "bridge","nat"}
docker compose up -d
```

## Services

| Name | Description | URL | Username | Password |
|-|-|-|-|-|
| CTFd | Challenges | [http://localhost:8000](http://localhost:8000) | alice | alice |
| Jenkins | CI Pipelines | [http://localhost:8080](http://localhost:8080) | alice | alice |
| Gitea | GitHub like | [http://localhost:3000](http://localhost:3000) | thealice | thealice |


## Hints

- [ Top 10 CI/CD Security Risks ](https://www.cidersecurity.io/top-10-cicd-security-risks/)
- [Jenkins - Handling credentials](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#handling-credentials)
- [Jenkins - String interpolation](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#string-interpolation)
- [How to grep (search through) committed code in the Git history](https://stackoverflow.com/questions/2928584/how-to-grep-search-through-committed-code-in-the-git-history)
- [Gitea - Clone repo using Token](https://forum.gitea.com/t/solved-clone-repo-using-token/1816)
- [Jenkins - Pipeline Syntax agent](https://www.jenkins.io/doc/book/pipeline/syntax/#agent)