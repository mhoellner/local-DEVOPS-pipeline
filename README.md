# Local DEVOPS Pipeline

A containerised build pipeline for C\#/.NET Core using the following tools:

* SonarQube
* Build Environment

The following tools are planned:

* TeamCity + Build Agents
* a private Docker registry (Artifactory has no free tier...), maybe Harbor + Image Scanning
* Gitlab CE + Gitlab Runner

## Setup

After cloning the repository, just run:
```bash
docker-compose up
```