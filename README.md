# Microservices Architecture


## Diagram Displaying Containerisation Vs Virtualisation

![image](https://user-images.githubusercontent.com/97620055/189631914-d7955ea6-32ca-44fc-ab7a-91a5b2a6b22c.png)



## What Is Containerisation And Docker ?

   - Containerisation --> is a form of virtualization where applications run in isolated user spaces, called containers, while using the same shared operating system (OS).
    
   - Docker --> a software platform simplifies the process of building, running, managing and distributing applications. It does this by virtualizing the operating system of the computer on which it is installed and running.

## Benefits of Containerisation and Docker:

* Advantages of Containerisation >
  i. Portability --> Write once and run anywhere (dependencies already present, so can take and build app anywhere)
  ii. Efficiency -->  use all available resources and minimise overhead (memory). 
  iii. Agility -->
  iv. Faster delivery -->
  v. Improved security -->
  vi. Faster app startup -->
  viii. Easier management -->
  ix. Flexibility -->

* Limitations of Containersation >
### Docker Pros:

  1. **Multi-application compatability** >  as long same OS, multiple applicatons with different requirements and dependencies can be hosted together on same host. 
  2. **Storage Optimized** > Occupy little disk space (few megabytes), so can have large number of applications can be hosted on the same host.
  3. **Robustness** > container does not have OS installed or running on it, so boot time is faster as compared to virtual machines.  
  4. **Reduces costs**. Docker is less demanding when it comes to the hardware required to run it.

### Docker Cons:

  1. Does not provide cross-platform compatability.
  2. Secuirty risks > if you are moving multiple pieces within  large scale dynamic docker enviromnent.
  3. Limitation on running applications with graphical interfaces > docker designed for hosting applications on command line interface. So need richer interfaces for applications with graphical interfaces. 
  4. Data recovery and strategy required > this is needed in case docker container goes down. Solutions are in place but not very scalable and are not automated.
   
### Docker Setup & Installation

- Click [here](https://docs.docker.com/desktop/install/windows-install/) to follow guidelines. Download and run  `exe ` file.
- Ensure on windows WSL2 latest update is configured from power shell as per guide above.
- Setup Docker Account.
- Restart maybe required a few times.

## Docker Best Practices:

### Docker (REST API) Commands:

- `docker build` - call indicates building of image.
- `docker run hello-world(:Specify version)` - if you do not specify version it automatically registers this as latest.  
- `docker run -p 80:80 nginx` - Running process from specified local host port (80) to (80) port on browser.
- `docker run -d -p 80:80 nginx` - the `d` specfies running process in background as in detached.
- `docker pull` - pulling image from dockerhub repository
- `docker images` - displays all the available docker images.
- `docker ps` - displays current state of running containers
- `docker ps -a` - displays all states of docker containers. 
  

### Docker Lifecycle Commands

- Ensure that the following commands below are run in exact order when maintaining lifecyle of docker containers. 
  
  - `docker start (containerid) -f` - force starts the container.
  - `docker stop (container id)-f` - force stops the container
  - `docker rm (container id) - f` - deletes the specified container
  - `docker rmi (image name) - f` > - deletes the specified docker image. 

### Executing Into Container Shell

- To enter into sheel of specific container please execute the following command: `docker exec -it (container-id) bash/sh`
- If any TTY errors, set up alias environment as follows in windows: `alias docker="winpty docker"`.

## Re-building Docker Images - Example (NGINX)

- Since docker images are immutable, we need to make changes and rebuild images inorder to save them. I have showed an example below in a few steps on how to do this.
  
    1. Create a new repository on docker hub, I called this `images`.
    2. Run this command > `docker run -d -p 90:80 nginx` - This automaticall pulls latest nginx image from docker hub registry and intialises this on port 90 of local host. 
    3. Check that page is running on local host as seen below

    4. Now to make changes, enter into docker container using exec shell command as above. 
    5. Navigate to `/usr/share/nginx/html`. 
    6. Ensure to perform in the order specified: `apt-get upate / apt install sudo / apt install nano`.
    7. Enter into index.html file by doing `sudo nano index.html`. Make relevant changes. Save and exit
    8. Exit the shell with `exit` command.
    9. Commit the changes to docker hub with command: `docker commit container-id   username/reponame:version-of-image`
   10. Push changes to docker hub repository using this command. `docker push container-id   username/reponame:version-of-image`
   11. Verify the changes has been made by going to `localhost:90`. 
  
  ![Haider's DevOps](https://user-images.githubusercontent.com/97620055/189713989-67a769a4-d32e-442a-aa9d-a0d6499c0bff.PNG)

  - This new image created as it is public is open for anyone to pull and run with following command on their local host: `docker run -d -p 90:80 haideralp/images:v1`. 

## Copying Data From Local Host --> Container:

  - Run nginx
  - Create a local directory using `mkdir` called docker. Create `index.html` file using `nano`.
  - You can use bootstrap to help with html templates but for simplicity I used



## Automate the Building of Image

- To automate the building of your image, you need to create a docker file as below and execute it. 

``` yaml
# Docker file - Creation
# select base image
FROM nginx:latest

# label it
LABEL Owner="haider.abedi@gmx.com"

# copy data from localhost to the container
COPY index.html /usr/share/nginx/html 

# allow required port
EXPOSE 80

# execute required command

CMD ["nginx","-g", "daemon off;"]

```
- Once this is done run the following commands as follows:
  ~
   * `docker build -t haideralp/nginx-webhosting .`
   * `docker run -d -p 80:80 haideralp/nginx-webhosting`
   * Verify by running `docker ps`.


## Building An Image for Node App

- Create a micro-service for node-app
- Create a dockerfile inside your app folder
- Create a script to package our node app in an iumage
- Create a container of our image
- Should load it on port 3000 or 80.
- Push it to docker up.
## Hosting Static Website on Docker



## Docker Compose Task

- After building the node js from the docker file. I have created docker compose file as below:

``` yaml
version: "3.9"  
services:              #defining the services as in containers that needs be summoned using this task. 
  mongodb:
    container_name: mongodb   #naming my container here
    image: mongo:3.6
    ports:
      - 27017:27017
    restart: always
    volumes:
      - ./mongod.conf:/etc/mongod.conf  # specifying volumes to ensure environment stays consistent. 
     
  
  app:
    container_name: nodeapp
    restart: always
    build: ./app/Dockerfile # defining path  where to build from (location of docker file)
    image: haideralp/nodeapp # image naming convention 
    ports:
      - 80:3000  #Specifying ports on which app needs to run on.
    depends_on: 
      - mongodb #giving condition that app is dependent on service mongodb running successfully. 
    environment:
      DB_HOST: mongodb://mongodb:27017     #setting of environment variables in the app.
    command: node app seeds/seed.js   # executing commands to fetch/seed data 
```
