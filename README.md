# docker
docker configurations

>Note: Containers need a **`container runtime`** to run as otherwise how will the containers run?

## 🐞 Basic Docker Commands 
```bash
# Create a container from the ubuntu image (with a name and WITHOUT the --rm flag)
docker run -it --name my-ubuntu-container ubuntu:22.04

# List all containers
docker container ps -a | grep name_of_container
docker container inspect my-ubuntu-container

# Restart the container and attach to running shell
docker start my-ubuntu-container
docker attach my-ubuntu-container

# --rm: This flag tells Docker to automatically remove the container after it exits.
# This is useful for cleaning up containers that are no longer needed to avoid cluttering your system. 
docker run -it --rm ubuntu:22.04

```

## 💾 Volume Mounts
- When you create a `Volume` and run a container and attach this created __Volume__ to the running container
- After performing or storing some data in that volume when you exit / delete the container, the data still persists in that volume
- You can launch another container and attach the same volume to this and __*Wallah!*__ you got your `Data` back 
- So, in short we can use volumes and mounts to safely persist the data.
- All of these `Volumes` lives inside the `Linux VM` that the Docker launches to run the containers as **THE CONTAINERS ARE LINUX**
```bash
# create a named volume
docker volume create my-volume

# Create a container and mount the volume into the container filesystem
docker run  -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
# There is a similar (but shorter) syntax using -v which accomplishes the same
docker run  -it --rm -v my-volume:/my-data ubuntu:22.04

# Now we can create and store the file into the location we mounted the volume
echo "Hello from the container!" > /my-data/hello.txt
cat my-data/hello.txt
exit

# Create a new container and mount the volume into the container filesystem
# Here, the data will be the same as above when the bove container exited
docker run  -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
cat my-data/hello.txt # This time it succeeds! 
exit
```

## 💻 Bind Mounts
- Alternatively, we can mount a directory from the `Host system` using a bind mount
  
```bash
# Create a container that mounts a directory from the host filesystem into the container
docker run  -it --rm --mount type=bind,source="${PWD}"/my-data,destination=/my-data ubuntu:22.04
# Again, there is a similar (but shorter) syntax using -v which accomplishes the same..
# PWD is the Present Working Directory in Host machine which is my Laptop and there is a directory inside PWD my-data
docker run  -it --rm -v ${PWD}/my-data:/my-data ubuntu:22.04

echo "Hello from the container!" > /my-data/hello.txt

# You should also be able to see the hello.txt file on your host system
cat my-data/hello.txt
exit
```





