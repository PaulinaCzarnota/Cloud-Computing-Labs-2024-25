# Cloud Computing - Lab 5 Solutions


## DevOps with Docker - Part 1

### Getting Started

In this week’s lab session, we dive into Docker containerization. By the end of the lab, you'll be able to:

- Run containerized applications.
- Create your own containerized applications.
- Use Docker volumes to store data persistently outside containers.
- Enable TCP access to containerized applications using port mapping.
- Share your containers publicly on Docker Hub.

### DevOps with Docker

We will work through Part 1 of the DevOps with Docker practical course from the University of Helsinki. The exercises in this section will help you understand how to work with Docker containers, from setting up basic environments to more advanced tasks like port mapping and creating Dockerfiles.

We will be using the Google Cloud Shell as our Linux terminal, which provides a convenient way to follow along and complete the exercises.

---

### Introduction

**Docker** is a tool used for creating and running containers. Containers package applications along with their dependencies, allowing them to run consistently across different environments. This section will guide you through running and managing containers, and understanding the difference between images and containers.

---

## Docker Basics

### Definitions

- **Docker Image**: A Docker image is a snapshot of a container. It contains the application and its dependencies. Docker images are immutable.
  
- **Docker Container**: A container is a running instance of an image. It is isolated and has its own file system, network interface, and resources.
  
- **Dockerfile**: A Dockerfile is a script that contains instructions for building a Docker image. It typically starts with a base image and adds layers (dependencies, files, configurations).

### Running Your First Container

To verify that Docker is correctly installed, run the following command:

```bash
docker run hello-world
```

This will pull the `hello-world` image from Docker Hub and run a container that outputs a message confirming that Docker is working correctly.

---

### Docker Commands Overview

Here are some commonly used Docker commands:

- `docker image ls`: List all Docker images.
- `docker container ls`: List all running containers.
- `docker container ls -a`: List all containers, including stopped ones.
- `docker run <image>`: Run a container from an image.
- `docker container stop <container>`: Stop a running container.
- `docker container rm <container>`: Remove a stopped container.
- `docker image rm <image>`: Remove an image.
- `docker ps`: Alias for `docker container ls`.

---

## Exercises

### Exercise 1.1: Getting Started

**Objective**: Start 3 containers from an image that does not automatically exit (such as `nginx`) in detached mode. Then stop two of the containers and leave one container running.

1. Run 3 `nginx` containers in detached mode:

   ```bash
   docker run -d nginx
   docker run -d nginx
   docker run -d nginx
   ```

2. List all containers to verify they are running:

   ```bash
   docker container ls -a
   ```

3. Stop two of the containers and leave one running:

   ```bash
   docker container stop <container_id_1>
   docker container stop <container_id_2>
   ```

4. Verify that one container is still running and two are stopped:

   ```bash
   docker container ls -a
   ```

**Solution**: The output of `docker container ls -a` should show two stopped containers and one running. Here's an example output:

```bash
CONTAINER ID   IMAGE    COMMAND                  CREATED        STATUS       PORTS     NAMES
abcd1234efgh   nginx    "nginx -g 'daemon of…'"   2 hours ago   Up 2 minutes 80/tcp   hopeful_wilson
efgh5678ijkl   nginx    "nginx -g 'daemon of…'"   2 hours ago   Exited (0)    2 minutes ago       elegant_heisenberg
ijkl7890mnop   nginx    "nginx -g 'daemon of…'"   2 hours ago   Exited (0)    2 minutes ago       kind_hamilton
```

### Exercise 1.2: Cleanup

**Objective**: Clean up Docker by removing all containers and images that are no longer in use.

1. List all containers:

   ```bash
   docker container ls -a
   ```

2. List all images:

   ```bash
   docker image ls
   ```

3. Remove all stopped containers:

   ```bash
   docker container prune
   ```

4. Remove all unused images:

   ```bash
   docker image prune
   ```

5. To remove everything (containers, networks, and unused images), use:

   ```bash
   docker system prune
   ```

6. Verify that there are no containers or images left:

   ```bash
   docker container ls -a
   docker image ls
   ```

**Solution**: After running the cleanup commands, the output for `docker container ls -a` and `docker image ls` should show no containers or images.

---

## Docker Best Practices

- **Minimize the number of layers in your Dockerfile**: Each command in a Dockerfile creates a new layer. Combine commands where possible to reduce the image size.
- **Use official images**: Official images from Docker Hub are optimized for security and performance.
- **Clean up unused containers and images**: Regularly prune unused containers and images to free up disk space.

---

## Running and Stopping Containers

### Introduction

In this section, we will explore how to run containers using more useful images and interact with them. We'll focus on using the `ubuntu` image, and learn several important Docker flags to interact with containers.

---

### Running Ubuntu in Docker

1. **Run Ubuntu Container**:
   You can start an Ubuntu container using the following command:

   ```bash
   docker run ubuntu
   ```

   If the image is not found locally, Docker will pull it from Docker Hub:

   ```bash
   Unable to find image 'ubuntu:latest' locally
   latest: Pulling from library/ubuntu
   83ee3a23efb7: Pull complete
   db98fc6f11f0: Pull complete
   f611acd52c6c: Pull complete
   Digest: sha256:703218c0465075f4425e58fac086e09e1de5c340b12976ab9eb8ad26615c3715
   Status: Downloaded newer image for ubuntu:latest
   ```

   However, this command won't interact with the container; it will exit right away.

2. **Run Ubuntu with TTY**:
   To interact with the container, we need to allocate a pseudo-TTY (`-t` flag):

   ```bash
   docker run -t ubuntu
   ```

   This opens the shell of the container, but the terminal is not interactive. We can't send input to it yet.

3. **Run Ubuntu Interactively**:
   To interact with the container, use the `-i` flag (interactive mode), which keeps the standard input open:

   ```bash
   docker run -it ubuntu
   ```

   Now, you are inside the container and can run commands. For example:

   ```bash
   ls
   ```

   This will display the directory contents inside the container:

   ```bash
   bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
   ```

---

### Running Containers in the Background

Now that we know how to interact with containers, let's run a container in the background.

1. **Run Container in Detached Mode**:

   To run a container in the background, use the `-d` flag (detached mode), along with `-it` to keep it interactive. For example, we will run a `looper` container that outputs the date every second:

   ```bash
   docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'
   ```

   The `--name looper` part assigns a name to the container, which makes it easier to reference. This command will start the container and run the specified script.

2. **Verify the Running Container**:

   To verify that the container is running, use:

   ```bash
   docker container ls
   ```

   This will list the running containers:

   ```bash
   CONTAINER ID   IMAGE    COMMAND                  CREATED        STATUS       PORTS     NAMES
   abc12345defg   ubuntu   "sh -c 'while true;..."   2 minutes ago   Up 2 minutes           looper
   ```

---

### Viewing Logs

To view the output of a running container, use the `docker logs` command:

```bash
docker logs -f looper
```

This will show the real-time output of the container (the current date being printed every second).

---

### Pausing and Unpausing Containers

You can pause the container without stopping it by using the `docker pause` command:

```bash
docker pause looper
```

This will freeze the container's processes, and the logs will stop updating. To unpause the container:

```bash
docker unpause looper
```

---

### Attaching to a Running Container

If you want to attach to a running container, use the `docker attach` command:

```bash
docker attach looper
```

This will connect you to the container's standard output. To disconnect from the container without stopping it, use the escape sequence `Ctrl + P` followed by `Ctrl + Q`.

---

### Starting and Re-attaching to a Container

You can start a stopped container with `docker start`, and then attach to it:

```bash
docker start looper
docker attach --no-stdin looper
```

The `--no-stdin` flag ensures that you don't close the container's process by sending a `Ctrl + C`.

---

### Executing Commands Inside a Running Container

If you need to run commands inside a running container, use `docker exec`. For example, to list all files inside the container:

```bash
docker exec looper ls -la
```

You can also start an interactive shell session:

```bash
docker exec -it looper bash
```

This will give you a Bash shell inside the container, where you can run commands as you would in a normal Ubuntu environment. For example:

```bash
ps aux
```

This shows the running processes in the container.

---

### Killing and Removing Containers

To stop a container immediately, use `docker kill`:

```bash
docker kill looper
```

To remove the container, use `docker rm`:

```bash
docker rm looper
```

Alternatively, to remove a container forcefully (even if it is running), use:

```bash
docker rm --force looper
```

---

### Running Containers with `--rm`

If you want a container to be automatically removed when it exits, use the `--rm` flag:

```bash
docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'
```

If you detach from the container (using `Ctrl + P`, `Ctrl + Q`), it will be removed once it exits. You can also attach to the container as usual:

```bash
docker attach looper-it
```

If you press `Ctrl + C`, it will stop the container and remove it.

---

## Exercise 1.3: Secret Message

**Objective**: Get inside a running container and follow its logs.

1. Run the `devopsdockeruh/simple-web-service:ubuntu` image. This container outputs logs to a file. Inside the running container, use `tail -f` to view the logs, which will print a "secret message" every 10 seconds.

   ```bash
   docker run -it devopsdockeruh/simple-web-service:ubuntu
   ```

2. Inside the container, use `tail` to follow the log file:

   ```bash
   tail -f ./text.log
   ```

3. Every 10 seconds, the container will print a secret message to the log.

---

## Exercise 1.4: Missing Dependencies

**Objective**: Start a container and identify missing dependencies.

1. Start a container with the following command:

   ```bash
   docker run -it ubuntu sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
   ```

2. You will notice that `curl` is not installed in the container. To install it, use the following commands inside the container:

   ```bash
   apt-get update
   apt-get -y install curl
   ```

3. After installing `curl`, test the container by entering a website like `helsinki.fi`:

   ```bash
   Input website: helsinki.fi
   Searching..
   ```

4. The output should display an HTML response from the website.

**Solution**: The command used to start the container is:

```bash
docker run -it ubuntu sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
```

To fix the missing dependencies, I installed `curl` using `apt-get`:

```bash
apt-get update
apt-get -y install curl
```

### In-Depth Dive into Docker Images

Docker images are the foundation of containerization, and understanding how they are built and utilized is crucial for working with containers effectively. In this detailed explanation, we'll break down key concepts related to Docker images, their sources, how to interact with them, and how to create your own.

#### Where Do Docker Images Come From?

When you run a Docker command like `docker run hello-world`, Docker first checks your local machine to see if the image `hello-world` exists. If it does not, Docker will pull the image from a remote repository, most commonly **Docker Hub**. Docker Hub is the default registry for Docker images, and it hosts both official images curated by Docker, Inc., and community-contributed images.

For example, running:
```bash
docker run postgres
```
will pull the latest official PostgreSQL image from Docker Hub. You can also search for images using the `docker search` command:
```bash
docker search hello-world
```
This command will return a list of images that match the search term. The output shows important details such as the image's name, description, the number of stars (community rating), and whether the image is "official" or "automated."

For example:
```bash
$ docker search hello-world
  NAME                         DESCRIPTION    STARS   OFFICIAL   AUTOMATED
  hello-world                  Hello World!…  1988     [OK]
  kitematic/hello-world-nginx  A light-weig…  153
  tutum/hello-world            Image to tes…  90       [OK]
```
- **Official Images** are curated by Docker, Inc., and are typically well-maintained. These images usually come from the `docker-library` repository.
- **Automated Images** are automatically built from a repository, often linked to a GitHub project.
- **Non-official Images** are community-contributed and may not be as rigorously maintained.

Additionally, Docker allows you to pull images from other registries, such as **Quay**. For example, to pull an image from Quay, you would use:
```bash
docker pull quay.io/nordstrom/hello-world
```
If you do not specify a registry, Docker will default to Docker Hub.

#### Examining a Docker Image

Let’s dive into a practical example by pulling and analyzing the **Ubuntu** image, one of the most commonly used base images for creating Docker containers.

To pull the image:
```bash
docker pull ubuntu
```
By default, this command pulls the `latest` tag. However, you can also pull a specific version, such as Ubuntu 22.04:
```bash
docker pull ubuntu:22.04
```
Images in Docker are made up of layers, and each layer is downloaded individually. This is done to speed up the process, as layers are cached locally and can be reused in future builds. Docker downloads multiple layers in parallel, as you can see when pulling Ubuntu:
```bash
$ docker pull ubuntu:22.04
  22.04: Pulling from library/ubuntu
  c2ca09a1934b: Downloading [==============================> ] 34.25MB/38.64MB
  d6c3619d2153: Download complete
  0efe07335a04: Download complete
  6b1bb01b3a3b: Download complete
  43a98c187399: Download complete
```
Each line here represents a separate layer in the image, and these layers are downloaded as needed. Docker uses caching to avoid downloading the same layers multiple times.

#### Tags and Versioning

Images are typically tagged to specify which version to use. For example:
```bash
docker pull ubuntu:22.04
```
In this case, `22.04` is the tag. Tags help identify different versions or builds of an image. By default, if no tag is specified, Docker will pull the `latest` tag. You can also give images custom tags:
```bash
docker tag ubuntu:22.04 ubuntu:jammy_jellyfish
```
Now, the tag `ubuntu:jammy_jellyfish` refers to the same image as `ubuntu:22.04`.

#### Creating and Building Your Own Docker Image

The next step in understanding Docker images is creating your own. This is done using a **Dockerfile**, a script that contains a series of instructions on how to build a Docker image.

Let's start by creating a simple shell script (`hello.sh`):
```bash
#!/bin/sh
echo "Hello, Docker!"
```
First, let's test the script locally:
```bash
chmod +x hello.sh
./hello.sh
```
Now we can create a Dockerfile to build an image based on **Alpine**, a small Linux distribution. The Dockerfile will copy the `hello.sh` script into the image and execute it.

Here's the Dockerfile:
```Dockerfile
# Use Alpine as the base image
FROM alpine:3.19

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy the hello.sh file into the container
COPY hello.sh .

# Make the script executable (if necessary)
RUN chmod +x hello.sh

# Define the default command to run the script
CMD ./hello.sh
```
With the Dockerfile created, we can build the image:
```bash
docker build . -t hello-docker
```
This command tells Docker to build an image from the current directory (`.`), using the instructions in the Dockerfile, and tag it as `hello-docker`. The output of the build process will show the steps as Docker creates layers for the image.

Once the image is built, you can run it:
```bash
docker run hello-docker
```
This will execute the `hello.sh` script inside the container, printing "Hello, Docker!" to the terminal.

#### Layers and Caching

Docker images are built in layers. Each instruction in the Dockerfile creates a new layer. For example, the `FROM` instruction creates the first layer, and the `COPY` instruction adds another layer with the contents of `hello.sh`. Docker uses a **build cache** to optimize future builds by reusing layers that haven’t changed. This can speed up build times.

For example, if we modify only the `hello.sh` script, Docker will reuse all the previous layers (such as the Alpine base image) and only rebuild the layer related to the script:
```bash
docker build . -t hello-docker
```
You can also manually create new layers by adding new files to the container or running additional commands using `RUN`.

#### Committing Changes to a Container

Once a container is running, you can modify its contents (e.g., adding a new file), and then **commit** the changes to create a new image. However, this approach is less efficient than modifying the Dockerfile directly. Here's how to commit changes:

1. Run the container:
   ```bash
   docker run -it hello-docker sh
   ```
2. Make changes inside the container (e.g., create a new file):
   ```bash
   touch additional.txt
   ```
3. Exit the container and commit the changes:
   ```bash
   docker commit <container-id> hello-docker-additional
   ```

However, it's a best practice to modify the Dockerfile to reflect changes, rather than committing changes to a running container.

#### Building an Image with a Script

Now, let's apply what we’ve learned to an exercise. Here’s how you can build an image for a script that runs indefinitely, asking the user for a website and then running `curl` to fetch the website's content.

1. Create a script `script.sh`:
```bash
while true
do
  echo "Input website:"
  read website
  echo "Searching..."
  sleep 1
  curl http://$website
done
```
2. Write the Dockerfile:
```Dockerfile
FROM ubuntu:22.04

# Install curl
RUN apt-get update && apt-get install -y curl

# Set the working directory
WORKDIR /usr/src/app

# Copy the script to the container
COPY script.sh .

# Make the script executable
RUN chmod +x script.sh

# Run the script when the container starts
CMD ./script.sh
```
3. Build the image:
```bash
docker build -t curler .
```
4. Run the container:
```bash
docker run -it curler
```
The container will start, prompting the user to input a website. It will then fetch and display the website's HTML.

To summarize the steps and lessons learned in your provided example, it appears you're building a Docker image with **yt-dlp**, a tool for downloading videos from platforms like YouTube, and using it within a Docker container. The approach involves creating a Dockerfile, testing changes interactively, and addressing problems with the container such as persisting downloaded files and increasing image size. Below is an improved process flow and explanation of what happens in each part.

### 1. Building the yt-dlp Docker Image
You start by creating a Docker image that installs **yt-dlp** to download videos from YouTube (and other sites). Let's break it down:

- **Initial image**: Start from the `ubuntu:22.04` base image.
- **Install dependencies**: First, you need to install `curl` and `python3` since `yt-dlp` needs Python 3.8+ and `curl` for fetching the downloader binary.
- **Download yt-dlp**: Use `curl` to download the latest yt-dlp binary and place it in `/usr/local/bin/yt-dlp`.
- **Make executable**: Ensure that the binary is executable with the `chmod a+x` command.

At this stage, the **Dockerfile** looks like this:

```dockerfile
FROM ubuntu:22.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python3
RUN curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
RUN chmod a+x /usr/local/bin/yt-dlp

ENTRYPOINT ["/usr/local/bin/yt-dlp"]
```

### 2. Running the Docker Image

After building the image, you attempt to run it using `docker run`. However, you face an issue where the URL you try to provide is being interpreted as a command, not an argument. 

- The problem is because of how Docker treats `CMD` and `ENTRYPOINT`:
  - `ENTRYPOINT` specifies the executable to run, while `CMD` provides default arguments. If only `ENTRYPOINT` is set, Docker will treat anything passed to `docker run` as a part of the executable command.

#### Fixing the issue using `ENTRYPOINT` and `CMD`

You can fix this by defining an `ENTRYPOINT` and a `CMD` in the Dockerfile:

```dockerfile
FROM ubuntu:22.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python3
RUN curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
RUN chmod a+x /usr/local/bin/yt-dlp

ENTRYPOINT ["/usr/local/bin/yt-dlp"]
CMD ["https://www.youtube.com/watch?v=Aa55RKWZxxI"]
```

This means:
- `ENTRYPOINT` will always be `/usr/local/bin/yt-dlp`, i.e., the video downloader.
- `CMD` provides the default URL to download. If you run `docker run yt-dlp`, it will download the video from this URL. If you specify a different URL when running the container, it will override this default.

#### Example:

```bash
$ docker run yt-dlp  https://www.youtube.com/watch?v=uTZSILGTskA
```

This will override the default URL (`CMD`) and download the video from the new URL.

### 3. Keeping Downloaded Files Outside the Container

The major problem with your current approach is that the downloaded files stay inside the container. When the container stops or is removed, the files are lost. There are several solutions for this:

1. **Mount a volume**: Use Docker's volume mechanism to persist data outside the container.
   
   Example:
   ```bash
   docker run -v /path/on/host:/mydir yt-dlp https://www.youtube.com/watch?v=uTZSILGTskA
   ```

   This will map the `/mydir` directory in the container to `/path/on/host` on the host machine, so the downloaded videos will be saved on your local machine.

2. **Copy files out manually**: As you demonstrated, you can use `docker cp` to copy files from a container to the host. However, this is less convenient than using volumes.

### 4. Optimizing Docker Image Layers

A minor problem you mentioned is the increased image size due to multiple `RUN` commands in the Dockerfile. You can reduce the number of layers by combining commands using `&&`:

```dockerfile
FROM ubuntu:22.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python3 && \
    curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && \
    chmod a+x /usr/local/bin/yt-dlp

ENTRYPOINT ["/usr/local/bin/yt-dlp"]
CMD ["https://www.youtube.com/watch?v=Aa55RKWZxxI"]
```

This way, the Docker image will have fewer layers, which can reduce its size and speed up builds.

### 5. Bonus: Improving the "Curler" Script

You also demonstrated how to use an entrypoint script (such as `curler.sh`) to make your container more flexible. Here's how you might modify a script to accept arguments:

1. Modify the script to use the first argument as input:
   
   ```bash
   #!/bin/bash

   echo "Searching.."
   sleep 1
   curl http://$1
   ```

2. Use `ENTRYPOINT` to make the script executable, and `CMD` to provide a default argument:

   ```dockerfile
   FROM ubuntu:22.04

   COPY curler.sh /usr/local/bin/curler.sh
   RUN chmod +x /usr/local/bin/curler.sh

   ENTRYPOINT ["/usr/local/bin/curler.sh"]
   CMD ["helsinki.fi"]
   ```

   Now, running `docker run curler-v2` will use the default argument (`helsinki.fi`), but you can override it:

   ```bash
   docker run curler-v2 example.com
   ```

---

## Interacting with the container via volumes and ports

## Exercise 1.9: Volumes

In this exercise, we will use Docker volumes to persist data from a container to the host machine. Specifically, we'll mount a local directory to the container and allow logs to be written to that directory, ensuring they are accessible even if the container is stopped or removed.

### Task

Use the `devopsdockeruh/simple-web-service` Docker image, which creates a timestamp every two seconds to `/usr/src/app/text.log` when it's not given any command. Start the container with a bind mount so that the logs are created into your filesystem (the host machine).

### Solution

To achieve this, we will use Docker's `-v` option to bind mount a host directory into the container. This will allow the log file `text.log` to be written directly to the host filesystem, instead of staying inside the container.

#### Step 1: Mount the current directory to the container's log location

We will run the following command to bind mount the current working directory to the `/usr/src/app` directory inside the container.

```bash
docker run -v "$(pwd)":/usr/src/app devopsdockeruh/simple-web-service
```

#### Explanation:

- `-v "$(pwd)":/usr/src/app`: This mounts the current directory (`$(pwd)`) on the host to `/usr/src/app` inside the container.
- `devopsdockeruh/simple-web-service`: This is the image we are using, which will create the `text.log` file in the `/usr/src/app` directory.

With this command, any logs generated inside the container will be saved to a `text.log` file in your current working directory.

---

## Exercise 1.10: Ports Open

In this exercise, we will expose a port from a Docker container to the host machine, allowing us to access the web service running inside the container from our browser.

### Task

The `devopsdockeruh/simple-web-service` image starts a web service on port `8080` when given the argument `server`. Use the `-p` flag to access the contents of the web service in your browser.

### Solution

To achieve this, we need to expose the container's port 8080 to the host machine and ensure that it is accessible from a browser.

#### Step 1: Expose and publish port 8080

Run the following command to expose port 8080 on the host machine, mapping it to port 8080 inside the container.

```bash
docker run -p 8080:8080 devopsdockeruh/simple-web-service server
```

#### Explanation:

- `-p 8080:8080`: This option maps port `8080` on the host to port `8080` inside the container. This allows you to access the container's web service from the host machine.
- `devopsdockeruh/simple-web-service server`: This command starts the container with the `server` argument, which begins the web service on port 8080 inside the container.

Once the container is running, you can open a web browser and go to `http://localhost:8080`. You should see a message similar to:

```json
{ "message": "You connected to the following path: ..." }
```

This confirms that the web service inside the container is running and accessible from the host machine.

---

## Summary of Commands

For **Exercise 1.9**, the command used to mount a directory and persist the logs:

```bash
docker run -v "$(pwd)":/usr/src/app devopsdockeruh/simple-web-service
```

For **Exercise 1.10**, the command used to expose the container's port and access it in a browser:

```bash
docker run -p 8080:8080 devopsdockeruh/simple-web-service server
```

---

### Additional Notes:

- **Bind Mounts**: The `-v` option is used for bind mounts, where you mount a directory or file from your local system into the container. This is especially useful for persisting data like logs or configuration files that should remain available even if the container is stopped or deleted.
  
- **Port Exposure and Publishing**: The `-p` option is used to expose and publish ports. This allows services running inside a container to be accessed from the outside world. In this case, mapping port 8080 from the container to port 8080 on the host makes the web service accessible from the browser at `http://localhost:8080`.

---

## Utilizing Tools from the Docker Registry

In this section, we will focus on how to transform project setup instructions into a Dockerfile and utilize tools from the Docker registry to containerize different types of applications.

---

## Exercise 1.11: Spring - Dockerfile for Java Spring Project

### Task

Create a Dockerfile for an old Java Spring project that can be found from the course repository. Follow the instructions in the README and utilize the appropriate Java version, using the Docker Hub `amazoncorretto` image.

### Solution

You can create a Dockerfile for a Java Spring project using the `amazoncorretto` image, which is a popular base image for running Java applications.

#### Step 1: Dockerfile Setup

```dockerfile
# Use the Amazon Corretto base image for Java
FROM amazoncorretto:11

# Set the working directory
WORKDIR /usr/src/app

# Copy the necessary files (e.g., JAR file) into the container
COPY . .

# Expose the port that the Spring app will run on
EXPOSE 8080

# Run the Spring application (adjust to the correct JAR file name)
CMD ["java", "-jar", "your-app.jar"]
```

### Explanation

- **FROM amazoncorretto:11**: We are using Amazon Corretto as the base image for Java 11.
- **WORKDIR /usr/src/app**: Setting the working directory inside the container.
- **COPY . .**: Copying the project files into the container.
- **EXPOSE 8080**: Exposing the default Spring Boot port.
- **CMD ["java", "-jar", "your-app.jar"]**: Running the Java application (you'll need to replace `"your-app.jar"` with the correct JAR filename).

---

## Mandatory Exercise 1.12: Hello, Frontend!

### Task

Clone, fork, or download the project from [example-frontend](https://github.com/docker-hy/material-applications/tree/main/example-frontend). Create a Dockerfile for the project and ensure the container runs with port `5000` exposed and published. When successful, you should see a message at `http://localhost:5000`.

### Solution

#### Step 1: Create the Dockerfile

```dockerfile
# Use an official Node.js image as the base
FROM node:14

# Set the working directory
WORKDIR /usr/src/app

# Copy the package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the project files
COPY . .

# Expose the port that the frontend application will run on
EXPOSE 5000

# Command to start the application
CMD ["npm", "start"]
```

#### Explanation

- **FROM node:14**: Using the official Node.js image for version 14.
- **WORKDIR /usr/src/app**: Setting the working directory inside the container.
- **COPY package*.json ./**: Copying the `package.json` and `package-lock.json` files to install dependencies.
- **RUN npm install**: Installing all the project dependencies.
- **COPY . .**: Copying the rest of the project files into the container.
- **EXPOSE 5000**: Exposing port 5000 for the frontend app.
- **CMD ["npm", "start"]**: Running the command to start the frontend app.

#### Step 2: Running the Docker Container

If you are on macOS with a newer version that reserves port `5000`, you can use another port, like `5001`.

```bash
docker build -t example-frontend .
docker run -p 5001:5000 example-frontend
```

After the container starts, navigate to `http://localhost:5001` (or `5000`, depending on your setup) to see the frontend in action.

---

## Mandatory Exercise 1.13: Hello, Backend!

### Task

Clone, fork, or download the project from [example-backend](https://github.com/docker-hy/material-applications/tree/main/example-backend). Create a Dockerfile for the backend project, and run the container with port `8080` published. When successful, you should be able to visit `http://localhost:8080/ping` and receive a "pong" response.

### Solution

#### Step 1: Create the Dockerfile

```dockerfile
# Use the official OpenJDK image for Java
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /usr/src/app

# Copy the necessary files (e.g., JAR file) into the container
COPY . .

# Expose port 8080
EXPOSE 8080

# Run the backend application (adjust to the correct JAR file name)
CMD ["java", "-jar", "your-backend-app.jar"]
```

#### Explanation

- **FROM openjdk:11-jre-slim**: Using the official OpenJDK image for Java 11 in a slim version.
- **WORKDIR /usr/src/app**: Setting the working directory.
- **COPY . .**: Copying the backend project files into the container.
- **EXPOSE 8080**: Exposing port 8080 for the backend service.
- **CMD ["java", "-jar", "your-backend-app.jar"]**: Running the backend service (replace `"your-backend-app.jar"` with the correct JAR filename).

#### Step 2: Running the Docker Container

```bash
docker build -t example-backend .
docker run -p 8080:8080 example-backend
```

Navigate to `http://localhost:8080/ping` to see the "pong" response.

---

## Mandatory Exercise 1.14: Environment Configuration

### Task

Start both the frontend and the backend projects with the correct ports exposed. Add `ENV` variables to the Dockerfiles with necessary information from both the frontend and backend READMEs. The frontend should send requests to `_backend_url_/ping` when you press a button.

### Solution

#### Step 1: Update Frontend Dockerfile with ENV for Backend URL

```dockerfile
# Use the official Node.js image
FROM node:14

# Set working directory
WORKDIR /usr/src/app

# Set backend URL as an environment variable
ENV BACKEND_URL=http://example-backend:8080

# Copy and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the files
COPY . .

# Expose the frontend port
EXPOSE 5000

# Start the frontend app
CMD ["npm", "start"]
```

#### Step 2: Update Backend Dockerfile (if necessary)

Ensure the backend is ready to accept requests from the frontend:

```dockerfile
# Use OpenJDK for Java
FROM openjdk:11-jre-slim

WORKDIR /usr/src/app

COPY . .

# Expose port 8080
EXPOSE 8080

# Run the backend app
CMD ["java", "-jar", "your-backend-app.jar"]
```

#### Step 3: Running Both Containers

Use Docker Compose or `docker run` commands to start both containers with the correct environment variables.

Example Docker Compose file (`docker-compose.yml`):

```yaml
version: "3.7"
services:
  frontend:
    build: ./frontend
    ports:
      - "5001:5000"
  backend:
    build: ./backend
    ports:
      - "8080:8080"
```

Run the following command to start both services:

```bash
docker-compose up --build
```

Now, the frontend should communicate with the backend, and the frontend button should turn green when it successfully calls the backend.

---

## Summary of Commands

- **Frontend Dockerfile build**: `docker build -t example-frontend .`
- **Backend Dockerfile build**: `docker build -t example-backend .`
- **Running with Docker Compose**: `docker-compose up --build`

---

### Additional Tips:

- **M1/M2 Mac Users**: If you encounter issues while building the image (e.g., with native dependencies), use the `--platform linux/amd64` flag during `docker build`, like so:

```bash
docker build --platform linux/amd64 -t imagename .
```

- **Environment Variables**: Use the `ENV` instruction in Dockerfiles to configure environment-specific values (e.g., API URLs, secrets) without modifying the source code of the application.

- **Checking Logs**: Always check the logs of your containers by using `docker logs <container_id>` to troubleshoot issues.


## Part 1 Summary

In this lab, we learned about Docker containers and images, starting from a clean Ubuntu environment and manually installing necessary software through a "Dockerfile." Dockerizing applications involved configuring dependencies, ensuring no versioning issues, and building images that anyone could easily run. We also explored how understanding the architecture and reading documentation helped us make informed decisions. While starting and stopping containers manually can be tedious, tools like Docker Compose offer a more efficient way to manage multiple containers. 