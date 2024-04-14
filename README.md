# dockerNotes
Docker is a platform and tool that allows developers to develop, deploy, and run applications in containers. Here's a breakdown of its key components and functionalities:

1. **Containerization**: Docker utilizes containerization technology to package applications and their dependencies into standardized units called containers. Containers provide a consistent environment for applications to run, ensuring that they behave the same way across different environments.

2. **Docker Engine**: At the core of Docker is the Docker Engine, which is responsible for building, running, and managing containers. It includes the Docker daemon, which runs in the background, and the Docker CLI (Command Line Interface), which allows users to interact with Docker through commands.

3. **Dockerfile**: A Dockerfile is a text file that contains instructions for building a Docker image. These instructions specify the base image, environment variables, dependencies, and other configurations required to create the containerized application.

4. **Docker Image**: A Docker image is a lightweight, standalone, executable package that contains everything needed to run a piece of software, including the code, runtime, libraries, and dependencies. Images are built from Dockerfiles and can be stored in Docker registries like Docker Hub.

5. **Docker Container**: A Docker container is a running instance of a Docker image. Containers isolate applications from their environment, ensuring that they run consistently regardless of the underlying infrastructure. Multiple containers can run on the same host without interference.

6. **Docker Hub**: Docker Hub is a cloud-based repository where users can store, share, and distribute Docker images. It provides access to a vast library of pre-built images for various programming languages, frameworks, and software components.

7. **Docker Compose**: Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure the services, networks, and volumes required for the application, making it easier to manage complex deployments.

8. **Docker Swarm**: Docker Swarm is a native clustering and orchestration tool for Docker. It allows users to create and manage a cluster of Docker hosts, enabling high availability, scaling, and load balancing for containerized applications.

Overall, Docker simplifies the process of building, shipping, and running applications by encapsulating them in containers, which can be easily deployed across different environments, from development to production. It has become a fundamental tool in modern software development workflows, enabling faster and more efficient application delivery.

To delete Docker images and their associated layers from disk, you can use the `docker rmi` command (remove image) to delete the images, and then optionally use the `docker system prune` command to clean up any dangling or unused data, including unused volumes and networks. Here's how you can do it:

### 1. Delete Docker Images:

#### Delete a Single Image:
To delete a single Docker image, use the `docker rmi` command followed by the image's name or ID:

```bash
docker rmi <image_name_or_id>
```

Replace `<image_name_or_id>` with the name or ID of the image you want to delete.

#### Delete Multiple Images:
To delete multiple Docker images at once, specify multiple image names or IDs separated by spaces:

```bash
docker rmi <image_name_or_id_1> <image_name_or_id_2> ...
```

### 2. Clean Up Unused Data:

#### Prune Unused Data:
To clean up unused data, including dangling images (those not associated with any container) and other unused resources, you can use the `docker system prune` command:

```bash
docker system prune
```

This command will prompt you to confirm the deletion of all unused data. To skip the confirmation prompt, you can add the `-f` or `--force` flag:

```bash
docker system prune -f
```

### 3. Deleting Specific Unused Data:

#### Delete Unused Volumes:
If you only want to delete unused volumes, you can use the `docker volume prune` command:

```bash
docker volume prune
```

#### Delete Unused Networks:
If you only want to delete unused networks, you can use the `docker network prune` command:

```bash
docker network prune
```

### Note:
- Deleting Docker images and unused data permanently removes them from your system, so make sure you want to delete them before running these commands.
- Be cautious when using the `docker system prune` command, as it will delete all unused data, including dangling images, volumes, and networks. Review the list of items to be deleted before confirming the operation.
- Always double-check the list of images and other resources to be deleted before executing any cleanup commands to avoid accidental data loss.

By using these commands, you can effectively manage Docker images and clean up unused data from your system to free up disk space.


**To run an existing container created but not currently running, you can use the `docker start` command followed by the container's name or ID. Here's the syntax:**

```bash
docker start <container_name_or_id>
```

Replace `<container_name_or_id>` with the name or ID of the container you want to start.

For example, if you have a container named `my_container` that you want to start, you would run:

```bash
docker start my_container
```

If you don't know the container's name, you can find it by running:

```bash
docker ps -a
```

This command lists all containers, including stopped ones, along with their names and IDs. Once you have identified the container you want to start, you can use the `docker start` command to start it.

After starting the container, you can use other Docker commands, such as `docker exec`, to interact with it or `docker logs` to view its logs.

Q **When I delete a Docker image and recreate it with the same name, it appears that the image is restored to its original state from when it was initially created. Additionally, Docker retains the memory of the previously deleted image.**

When you delete a Docker image and then recreate it with the same name, Docker may still use cached layers from the previous image if they haven't changed. This behavior is due to Docker's layer caching mechanism, which improves build performance by reusing previously built layers when possible.

Here's why this might happen and how you can manage it:

1. **Layer Caching**: Docker uses a layered file system, where each instruction in a Dockerfile creates a layer. When you rebuild an image, Docker reuses existing layers if the instructions and context haven't changed. This means that if you delete an image and rebuild it with the same instructions, Docker may use cached layers from the previous build, resulting in a faster build process.

2. **Persistent Storage**: When you delete an image, Docker removes the image from your local image cache, but it doesn't necessarily delete the underlying layers from disk immediately. If you recreate the image with the same name, Docker may still find the cached layers on disk and reuse them.

3. **Managing Cache**: To force Docker to rebuild the image from scratch without using cached layers, you can use the `--no-cache` option with the `docker build` command. This option tells Docker to ignore the cache and build all layers from scratch.

   Example:
   ```bash
   docker build --no-cache -t your_image_name .
   ```

4. **Cleaning Up**: If you want to completely remove all traces of the deleted image, including cached layers, you can use the `docker system prune` command to clean up unused data, including cached images and layers.

   ```bash
   docker system prune
   ```

   Be cautious when using this command, as it will also remove other unused data, such as stopped containers and dangling images.

By understanding Docker's layer caching mechanism and managing cache appropriately, you can control the build process and ensure that your Docker images are created and rebuilt as expected.
