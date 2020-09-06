# Local DEVOPS Pipeline

A containerised build pipeline for C\#/.NET Core using the following tools:

* [SonarQube](https://hub.docker.com/_/sonarqube)
* Build Environment

The following tools are planned:

* [TeamCity Server](https://hub.docker.com/r/jetbrains/teamcity-server) + [Build Agents](https://hub.docker.com/r/jetbrains/teamcity-agent)
* a private Docker registry (Artifactory has no free tier...), maybe Harbor + Image Scanning
* [Gitlab CE](https://hub.docker.com/r/gitlab/gitlab-ce) + [Gitlab Runner](https://hub.docker.com/r/gitlab/gitlab-runner)

## Setup

After cloning the repository, just run:
```bash
docker-compose up sonarqube
```
The SonarQube server will be available on http://localhost:9000.

Next, create a token to be able to create projects in SonarQube.

Install the [C# Language Plugin](https://docs.sonarqube.org/latest/analysis/languages/csharp/).

## Usage
1. Build Build Environment  
`docker-compose build buildenv`
1. Start Build Environment  
`docker run --name buildenv -it --rm --network local-devops-pipeline_default buildenv-dotnet:3.1.200 bash`
1. Copy Application into Build Environment  
`docker cp ./src buildenv:/app`
1. Start SonarScanner   
`dotnet sonarscanner begin /key:Application /d:sonar.login="..." /d:sonar.host.url="http://sonarqube:9000" /d:sonar.cs.vstest.reportsPaths="${PWD}/TestResults/*.trx" /d:sonar.cs.opencover.reportsPaths="${PWD}/TestResults/**/coverage.opencover.xml"`
1. dotnet test  
`dotnet test --logger:trx --results-directory:${PWD}/TestResults --collect:"XPlat Code Coverage" --settings:coverlet.runsettings Application.sln`
1. Stop SonarScanner  
`dotnet sonarscanner end /d:sonar.login='...'`
