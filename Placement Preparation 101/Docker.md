---
status: To Review
last-reviewed: 2025-09-01
tags:
  - docker
---
#placement-preparation #docker 

> ref - https://www.youtube.com/watch?v=HR7wjLxQw1Q
### Docker Containers and Images
**Docker Containers**
 Docker containers are lightweight, standalone, executable packages that include everything needed to run a piece of software, such as the application code, runtime, libraries, and dependencies

They provide an isolated environment for applications, ensuring consistency across different environments (development, testing, production)

**Docker Images**
Docker images are immutable templates used to create containers. They serve as the "blueprint" for a container, containing the application code, environment variables, and configuration files
#### Virtual Machine
VMs use the concept of Hypervisor that divides the hardware resources (like memory, RAM, CPUs) to each individual OS
So, if you have 50 GB RAM, 100 GB storage and 12GB RAM then hypervisor will let you have 2 OS - Windows and Ubuntu and allow you to divide them in any proportion, say - 30 GB RAM, 60 GB Disk and 8GB RAM to ubuntu and remaining to windows
#### Docker vs Virtual Machine
Although both provide virtualization, VM contains its own OS and a kernel, whereas docker uses the host's OS and kernel - consuming much less resources
![[Docker vs VM|500]]
### Docker Flow
`docker images` - lists down all the images that you have
`docker run --name <name of Container> -d -p <port mapping> <image name>` - creates a new container with specified name, port from the provided image
	`-d` flag is used to run it silently in the background
`docker ps` - lists the running containers

### Dockerizing Spring Boot Application

```dockerfile
FROM maven:3.9.9-eclipse-temurin-21 AS builder
WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn clean package -DskipTests

FROM openjdk:21-jdk AS runner
WORKDIR /app

COPY --from=builder /app/target/*.jar ./app.jar

EXPOSE 9091

ENV DB_URL="jdbc:mysql://mysql:3306/rms"
ENV DB_USERNAME="root"
ENV DB_PASSWORD="sql_psarthak#16"
ENV SERVER_PORT=9091

ENTRYPOINT ["java", "-jar", "app.jar"]
```

1. Uses a Maven image with JDK 21 to compile the app. Named builder so we can reference it later.
2. Sets working directory inside the container to `/app`
3. Copies only pom.xml to root folder (`.`)
4. Runs `mvn dependency:go-offline` to download dependencies before copying source code → helps with build caching.
5. Copies source files to `/src` folder
6. Builds the JAR (`target/*.jar`) while skipping tests for faster builds
7. Lightweight JDK 21 image, used only to run the final JAR. Keeping the final image smaller since Maven isn’t needed here.
8. Set the working directory back to `/app`
9. Copies the built JAR from the builder stage into the runner stage
10. Declares that the app runs on port 9091 (for Docker networking)
11. Sets environment variables for database URL
12. Sets environment variables for database username
13. Sets environment variables for database password
14. Sets environment variables for database server port. (These can be overridden at runtime with `docker run -e`)
15. Defines the command to run the Spring Boot app when the container starts.
## Docker Commands
`docker pull <imagename>` - pulls the docker image from docker-hub
`docker push <imagename>` - pushes the docker image 
`docker run --name <nameOfContainer> <nameOfImage>` - creates a docker container with specified image
- -d : Run container in background and print container ID
- -t : allocate a pseudo-TTY
`docker ps`: lists down the running docker containers
- -a : lists the past/stopped one as well (provides history)
`docker rm <containerId>` - removes the docker container with specified container-id
`docker rmi <imageName>` - removes the docker image
`docker images` - lists down all the images in docker
`docker start|stop <containerName>` - starts/stops  created docker container
`docker exec -it <containerName> sh` - opens the shell inside a running container
`docker logs -f <containerName>` - fetch and follows container logs
`docker container stats` - view resource usage stats

![[Docker Cheat Sheet.pdf]]