# [Docker Sample App Tutorial](https://docs.docker.com/get-started/)
## Part 1: Getting Started
1. `docker run -d -p 80:80 docker/getting-started`
   1. `-d`: run the container in detached mode (in the background)
   2. `-p 80:80`: map port 80 of the host to port 80 in the container
   3. `docker/getting-started`: the image to use
   4. acceptable shorthand: `docker run -dp 80:80 docker/getting-started`
2. Opening the "Docker Dashboard" in the Docker app, you should now see an instance with a randomly generated name, and running on `PORT: 80`
## Part 2: Sample application
1. Add contents of `app` in [getting-started docker repo](https://github.com/docker/getting-started) to this repo
4. Add `DockerFile`
   1. [`FROM`](https://docs.docker.com/engine/reference/builder/#from)
      1. Required
      2. Installs the Docker Image: `node:12-alpine`
      3. In general, Docker's Official Images "provide essential base OS repos (e.g. ubuntu, centos) that serve as the starting point for the majority of users"
   2. [`WORKDIR`](https://docs.docker.com/engine/reference/builder/#workdir)
      1. sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile
   3. [`COPY . .`](https://docs.docker.com/engine/reference/builder/#copy)
      1. Copies new files or directories:
         1. **from** the first provided "src" path `.` (current directory on host machine)
         2. **to** the second provided "dest" path `.`, a path relative (no leading `/`) to `WORKDIR`: `/app`
   4. [`RUN`](https://docs.docker.com/engine/reference/builder/#run)
      1. Will execute any commands in a new layer on top of the current image and commit the results.
   5. [`CMD`](https://docs.docker.com/engine/reference/builder/#cmd)
      1. The main purpose of a CMD is to provide defaults for an executing container.
5. `docker build -t getting-started .`
   1. `docker build`: Build an image from a Dockerfile
   2. `-t getting-started`: name/tag image "getting-started"
   3. `.`: Look for `Dockerfile` in current dir
6. Start an app container
   1. `docker run -dp 3000:3000 getting-started`
      1. `docker run` (`docker help run`)
      2. `-df 3000:3000`
         1. `-d`: Run container in background and print container ID
         2. `-p`: Publish a container's port(s) to the host
      3. `getting-started`: This is the one required arg for `docker run` -- the image name
7. Go to `http://localhost:3000/` and add a few items to the list
8. Confirm in Docker Application that two containers are running
   1. The tutorial app container that you created at the very beginning with `docker run -dp 80:80 docker/getting-started`
   2. The launched application that you created with `docker run -dp 3000:3000 getting-started`
## Part 3: Update the Application
1. Update message rendered when no items have been added to `You have no todo items yet! Add one above!`.
2. Rebuild application with the updated version of the image:
   1. `docker build -t getting-started .`
3. Remove old container and add a new one can be added
   1. `docker ps`: To find image container ID
   2. `docker stop <the-container-id>`
   3. `docker rm <the-container-id>`
   4. `docker run -dp 3000:3000 getting-started`
4. Refresh your browser on http://localhost:3000 to see updated message