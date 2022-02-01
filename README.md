# About 
This is a .NET 5 project WebApi template to show the basics of Docker.

This readme also contains some summarized information about Docker and its commands based on my studies.

# Project technologies
* .NET 5
* REST API
* Docker

* <i>Prerequisites: .NET 5 Runtime, Visual Studio / Visual Studio Code, Docker Desktop <br />
If the OS is Windows, it has to have WSL2 installed and in some cases Virtualization enabled on BIOS. </i>

- When creating the WebApiProject template it's possible to 'Enable Docker Support' on Visual Studio (this project was done following this approach).  <br /> Otherwise, it's also possible to enable it with the project already created.

<div align="center">

<img align="center" alt="docker1" height="80" width="60" src="https://cdn.jsdelivr.net/npm/devicons@1.8.0/!SVG/docker.svg"/> 

<img align="center" alt="docker2" height="80" width="60" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" />

<img align="center" alt="docker1" height="80" width="60" src="https://cdn.jsdelivr.net/npm/devicons@1.8.0/!SVG/docker.svg"/> 

</div>

# Docker overview
<i>"Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production." </i>

On a DevOps context, Docker can be used on Development and Deployment steps.

The main goal is to eliminate the problem of <b><i>"it works perfectly on my machine"</i></b> and then in production the surprise... <b><i>it simply doesn't work</i></b>.

   &nbsp; [>>More information about Docker](https://docs.docker.com/get-started/overview/)

# Docker file
<i>Example of docker file below.</i>
* Containing .NET 5 image, SDK;
* Ports to be exposed;
* Folders of build and publish

```coffee
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/Docker.Basics.Api/Docker.Basics.Api.csproj", "src/Docker.Basics.Api/"]
RUN dotnet restore "src/Docker.Basics.Api/Docker.Basics.Api.csproj"
COPY . .
WORKDIR "/src/src/Docker.Basics.Api"
RUN dotnet build "Docker.Basics.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Docker.Basics.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Docker.Basics.Api.dll"]
```

# Useful docker basics commands
These commands can be used through Command Line Interface. 
In order to manipulate a specific project container, such as this one, the commands should be executed on the same folder where dockerfile is located.

- Check available images
```coffee
docker images
```

- Get containeres being executed
```coffee
docker ps
```

- Check local docker version
```coffee
docker -v
```

- Create local docker image
```coffee
docker build -t imageName:version
```

- Run local image
```coffee
docker run -p entrancePort:internalPort imageId
```

- Stop a container execution
```coffee
docker stop containerId
```

- Delete a local image
```coffee
docker rmi imageName
```

- Change an image tag name
```coffee
docker tag imageId yourDockerHubId/yourRepositoryName
```

# Docker compose
<i>"Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration."</i>

   &nbsp; [>> More information about Docker Compose](https://docs.docker.com/compose/)   

- Run docker compose file
```coffee
docker-compose up
```coffee

- Execute specified docker-compose file
```coffee
docker-compose -f docker-compose.yml
```

# Docker Hub
<i>"Docker Hub is a service provided by Docker for finding and sharing container images with your team. It is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers."</i>

   &nbsp; [>> More information about Docker Hub](https://docs.docker.com/docker-hub/)   

- Get image from Docker Hub
```coffee
docker pull redis
```

- Send local image to Docker Hub
```coffee
docker push yourDockerHubId/yourRepositoryName
```
