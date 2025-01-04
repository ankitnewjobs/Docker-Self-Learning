---
title: "How to Pull and Run Docker Images from Docker Hub and run"
description: "Learn how to pull Docker images from Docker Hub and run them. This guide covers pulling images, running containers, starting and stopping containers, and removing images."
---

# How to Pull and Run Docker Images from Docker Hub and Run

---

## Introduction

In this guide, you will:

1. **Pull Docker images** from Docker Hub.
2. **Run Docker containers** using the pulled images.
3. **Start and stop Docker containers**.
4. **Remove Docker images**.
5. **Important Note:** Docker Hub sign-in is not needed for downloading public images. In this case, the Docker image `stacksimplify/mynginx` is public and does not require authentication.

---

## Step 1: Pull Docker Image from Docker Hub

```bash
# List Docker images (should be empty if none are pulled yet)
docker images

# Pull Docker image from Docker Hub
docker pull stacksimplify/mynginx:v1

# Alternatively, pull from GitHub Packages (no download limits)
docker pull ghcr.io/stacksimplify/mynginx:v1

# List Docker images to confirm the image is pulled
docker images
```

**Important Notes:**

1. **Docker Image Pull Limits:** Docker Hub imposes pull rate limits for anonymous and free users.
2. **Alternative Registry:** To avoid hitting Docker Hub pull limits, you can pull the same Docker image from **GitHub Packages**.
3. **Consistency:** Both images are the same; choose either Docker Hub or GitHub Packages based on your needs.

---

## Step 2: Run the Downloaded Docker Image

- **Copy the Docker image name** from Docker Hub or GitHub Packages.
- **HOST_PORT:** The port number on your host machine where you want to receive traffic (e.g., `8080`).
- **CONTAINER_PORT:** The port number within the container that's listening for connections (e.g., `80`).

```bash
# Run Docker Container
docker run --name <CONTAINER-NAME> -p <HOST_PORT>:<CONTAINER_PORT> -d <IMAGE_NAME>:<TAG>

# Example using Docker Hub image:
docker run --name myapp1 -p 8080:80 -d stacksimplify/mynginx:v1

# Or using GitHub Packages image:
docker run --name myapp1 -p 8080:80 -d ghcr.io/stacksimplify/mynginx:v1
```

**Access the Application:**

- Open your browser and navigate to [http://localhost:8080/](http://localhost:8080/).

---

## Step 3: List Running Docker Containers

```bash
# List only running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# List only container IDs
docker ps -q
```

---

## Step 4: Connect to Docker Container Terminal

You can connect to the terminal of a running container to inspect or debug it:

```bash
# Connect to the container's terminal
docker exec -it <CONTAINER-NAME> /bin/sh

# Example:
docker exec -it myapp1 /bin/sh

# Inside the container, you can run commands:
ls
hostname
exit  # To exit the container's terminal
```

**Execute Commands Directly:**

```bash
# List directory contents inside the container
docker exec -it myapp1 ls

# Get the hostname of the container
docker exec -it myapp1 hostname

# Print environment variables
docker exec -it myapp1 printenv

# Check disk space usage
docker exec -it myapp1 df -h
```

---

## Step 5: Stop and Start Docker Containers

```bash
# Stop a running container
docker stop <CONTAINER-NAME>

# Example:
docker stop myapp1

# Verify the container has stopped
docker ps

# Test if the application is down
curl http://localhost:8080

# Start the stopped container
docker start <CONTAINER-NAME>

# Example:
docker start myapp1

# Verify the container is running
docker ps

# Test if the application is back up
curl http://localhost:8080
```

---

## Step 6: Remove Docker Containers

```bash
# Stop the container if it's still running
docker stop <CONTAINER-NAME>
docker stop myapp1

# Remove the container
docker rm <CONTAINER-NAME>
docker rm myapp1

# Or stop and remove the container in one command
docker rm -f <CONTAINER-NAME>
docker rm -f myapp1
```

---

## Step 7: Remove Docker Images

```bash
# List Docker images
docker images

# Remove Docker image using Image ID
docker rmi <IMAGE-ID>

# Example:
docker rmi abc12345def6

# Remove Docker image using Image Name and Tag
docker rmi <IMAGE-NAME>:<IMAGE-TAG>

# Example:
docker rmi stacksimplify/mynginx:v1
```

---

## Conclusion

You have successfully learned how to pull Docker images from Docker Hub or GitHub Packages, run containers from those images, interact with running containers, and manage containers and images on your local machine.

**Congratulations!**

---

## Additional Notes

- **Replace Placeholders:** Remember to replace `<CONTAINER-NAME>`, `<HOST_PORT>`, `<CONTAINER_PORT>`, `<IMAGE_NAME>`, and `<TAG>` with your actual values.
- **Docker Hub vs. GitHub Packages:** If you encounter Docker Hub pull rate limits, consider using GitHub Packages (`ghcr.io`) as an alternative.
- **Container Names:** Giving meaningful names to your containers helps in managing them easily.
- **Cleanup:** Regularly remove unused containers and images to free up disk space.

---

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Docker CLI Command Reference](https://docs.docker.com/engine/reference/commandline/docker/)

---


**Happy Dockering!**

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Explanation:- 

The provided content explains how to pull and run Docker images from Docker Hub and manage Docker containers and images on your local machine. 

Let's break it down for better understanding:

### 1. What is Docker Hub?

Docker Hub is a cloud-based registry that allows developers to store, share, and distribute container images. It includes public and private image repositories. 

Public images are accessible without authentication, while private images require authentication.

### 2. Pulling Docker Images

When you use docker pull, Docker downloads the specified image from Docker Hub (or any other registry). 

#### Command Example:

docker pull stacksimplify/mynginx:v1

This downloads the image stacksimplify/mynginx with the tag v1. You can verify the image is downloaded by listing the images:

docker images

Theoretical Insight:

- Images are read-only templates with instructions to create containers.
- Tags (e.g.: v1) represent specific versions of an image.
- Alternative registries like ghcr.io can be used to avoid Docker Hub rate limits.

### 3. Running Docker Containers

A Docker container is a runtime instance of an image. Using docker run, you can start a container from a pulled image.

#### Command Example:

docker run --name myapp1 -p 8080:80 -d stacksimplify/mynginx:v1

- --name: Assigns a name to the container (myapp1).
- -p 8080:80: Maps port 80 inside the container to port 8080 on the host machine.
- -d: Runs the container in detached mode (in the background).

Access the application by navigating to http://localhost:8080.

Theoretical Insight:

- Port mapping enables communication between the host and the container.
- Running containers in detached mode allows the terminal to remain free from other commands.

### 4. Listing Running Containers

Use docker ps to view active containers. To see all containers (including stopped ones), use:

docker ps -a

#### Command Output Example:

CONTAINER ID   IMAGE                    COMMAND                  STATUS         PORT                  NAMES
abc123         stacksimplify/mynginx   "nginx -g 'daemon of…"   Up 10 minutes  0.0.0.0:8080->80/tcp   myapp1

### 5. Connecting to a Container's Terminal

You can use docker exec to access a running container's shell for debugging or inspecting its state.

#### Command Example:

docker exec -it myapp1 /bin/sh

Inside the container, you can execute Linux commands like:

ls
hostname
exit

Theoretical Insight:

- This method allows real-time interaction with the container environment.
- Useful for troubleshooting or configuring the container dynamically.

### 6. Stopping and Starting Containers

You can stop a container to free up resources and restart it later without data loss.

#### Commands:

docker stop myapp1      # Stop the container
docker start myapp1     # Restart the stopped container

Test the Application: Use curl or a browser to check if it's running:

curl http://localhost:8080

### 7. Removing Containers

To clean up unused containers, stop them first (if running) and then remove them.

#### Commands:

docker rm myapp1            # Remove a stopped container
docker rm -f myapp1         # Force-remove a running container

### 8. Removing Docker Images

To free up disk space, remove images that are no longer in use.

#### Commands:

docker images               # List all images
docker rmi stacksimplify/mynginx:v1  # Remove image by name and tag
docker rmi <IMAGE-ID>       # Remove image by ID

### 9. Important Notes

- Always replace placeholders (e.g., <CONTAINER-NAME>) with actual values.
- Regularly clean up unused containers and images to optimize disk usage.
- Use alternative registries (e.g., ghcr.io) if Docker Hub limits your pull requests.

### Theoretical Context with Example

Let's consider a real-world use case where a company wants to deploy a web server (e.g., NGINX) using Docker.

1. Pull the NGINX image:
  
   docker pull nginx:latest
  

2. Run the NGINX container:
  
   docker run --name webserver -p 8080:80 -d nginx:latest
  

3. Access the website:

    Open http://localhost:8080 in a browser.

5. Stop and remove the container:
   
   docker stop webserver
   docker rm webserver
   

6. Remove the NGINX image:
   
   docker rmi nginx:latest
   
This example demonstrates the entire lifecycle of a Docker container, from pulling an image to running, stopping, and cleaning it up.
