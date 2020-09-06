FROM mcr.microsoft.com/dotnet/core/sdk:3.1.200-buster

# install Amazon Corretto 11 for SonarScanner
RUN apt update && \
    apt install -y software-properties-common && \
    wget -O- https://apt.corretto.aws/corretto.key | apt-key add - && \
    add-apt-repository 'deb https://apt.corretto.aws stable main' && \
    apt-get update && \
    apt-get install -y java-11-amazon-corretto-jdk

# install SonarScanner as .NET Core Global Tool
RUN dotnet tool install --global dotnet-sonarscanner
ENV PATH=${PATH}:/root/.dotnet/tools

WORKDIR /app
