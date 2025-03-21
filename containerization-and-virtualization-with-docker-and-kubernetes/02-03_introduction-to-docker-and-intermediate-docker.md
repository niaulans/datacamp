## [Introduction to Docker](https://app.datacamp.com/learn/courses/introduction-to-docker) and [Intermediate Docker](https://app.datacamp.com/learn/courses/intermediate-docker)

### Basic bash commands
```bash
nano <file_name>            # open file in nano editor
touch <file_name>           # create file
echo <text> > <file_name>   #  write text to file
<command> >> <file_name>    #  append text to file
<command> -y                #  automatic yes to prompts
```

### Docker CLI
```
- Docker command line interface will send commands to the Docker daemon
- every command starts with docker
```

### Run docker container
```bash
docker run <image_name> # run a container
docker run hello-world

docker run -it <image_name> # run a container in interactive mode
docker run -it ubuntu

docker run -d <image_name> # run a container in detached mode
docker run -d ubuntu
```

### Run named containers
```bash
docker run --name <container-name> <image-name>
docker run --name my-container ubuntu
docker run -d --name my-container ubuntu
```

 ### List running containers
 ```bash
docker ps     # list all running containers
docker ps -a  # list all containers
```

### Stop and remove containers
```bash
docker stop <container_id/container_name>         # stop a container
docker stop $(docker ps -aq)                      # stop all active and non-active containers
docker rm <container_id/container_name>           # remove a container
docker container rm <container_id/container_name> # remove a container (alternative)
docker rm -f $(docker ps -aq)                     # force remove all container 
docker container prune -a                         # remove all unused containers
```

### Filtering running containers
```bash
# Filter by name
docker ps -f "name=<container-name>"
docker ps -f "name=my-container"

# Container logs
docker logs <container-id>
docker logs my-container
docker logs -f my-container # follow logs
```

### Managing local docker Images
```bash 
# Pulling an image from Docker Hub
docker pull <image_name>
docker pull ubuntu
docker pull postgres

docker pull <image_name>:<image-version>
docker pull ubuntu:20.04
```

### Listing images
```bash
docker images
```

### Removing images
```bash
# Remove an image
docker rmi <image_id>
docker rmi ubuntu

# Remove unused images
docker image prune -a

# Remove all images
docker rmi -f $(docker images -aq)
```

### Distributing Docker Images
```
- Private Docker Registries -> store and distribute Docker Images
- Name starts with the url of the registry
- dockerhub.myprivateregistry.com/my-image
```

### Pull image from a private registry
```bash
docker pull <registry-url>/<image-name>:<tag>
```

### Name an image
```bash 
docker tag <old-image-name> <new-image-name>:<tag>
```

### Pushing to a specific registry
```bash
docker tag <image-name> <registry-url>/<image-name>:<tag>
docker image push <registry-url>/<image-name>:<tag>
```

```
Docker official images      -> No authentication required
Private Docker repositories ->  Owner can choose
docker login dockerhub.myprivateregistry.com
```

### Save an image
```bash
docker save -o <path-to-save> <image-name><tag>
```

### Load an image
```bash
docker load -i <path-to-image>
docker load -i my-image.tar
```

### Creating images with Dockerfile
```
Dockerfile -> Docker Images -> Docker Containers
           build           run

Starting a Dockerfile:
FROM postgres
FROM ubuntu
FROM hello-world
FROM my-custom-data-pipeline

Customizing images:
RUN <valid_command>

FROM ubuntu
RUN apt-get update
RUN apt-get install -y python3
```

### Building a Dockerfile
```bash
docker build /location/of/Dockerfile
docker build .
docker build -t <image-name> /location/of/Dockerfile
docker build -t my-image .
```

### Write command from bash to Dockerfile
```bash
echo "RUN apt-get update" >> Dockerfile # write to Dockerfile
```

### REMEMBER!!!
```
touch -> make new file
nano  -> open file in nano editor
cat   -> display file content
```

### COPYing files into an image 
```Dockerfile
COPY <source> <destination>
COPY /projects/pipeline_v3/pipeline.py /app/pipeline.py

# Can't copy init.py into an image.
```

### Downloading files
```Dockerfile
# Download files
RUN curl <file_url> -o <destination>

# Unzip files
RUN unzip <destination>/<file_name>.zip

# Remove the original zip file
RUN rm <destination>/<file_name>.zip

# Downloading files efficiently
RUN curl <file_url> -o <destination> \
    && unzip <destination>/<file_name>.zip \
    && rm <destination>/<file_name>.zip
```

### Pipeline Dockerfile
```Dockerfile
FROM ubuntu 

RUN apt-get update                                            
RUN apt-get install -y python3 curl unzip                     
RUN curl https://assets.datacamp.com/production/repositories/6082/datasets/31a5052c6a5424cbb8d939a7a6eff9311957e7d0/pipeline_final.zip -o /pipeline_final.zip 
RUN unzip /pipeline_final.zip                                 
RUN rm /pipeline_final.zip 
```

### Docker caching and layers 
```
- Docker layer: All changes by a single Dockerfile instruction
- Docker image: All layer created during a build

Docker caching:
- Consecutive builds are much faster because Docker re-use layers that haven't changed
- Docker can't know when a new version of python3 is released.
- Docker will use cached because the instructions are identical to previous builds.
```

### Changing users and working directory
```
WORKDIR : Change the working directory
USER    : Change the user
```

```Dockerfile
WORKDIR /home/my_user_with_a_long_name/work/projects/
COPY /projects/pipeline_v3/ app/
```

### Linux permissions
``` 
- Root is special user with all permissions

Best practices:
- Create a new user with limited permissions
- Stop using root user

FROM ubuntu -> root user
RUN apt-get update -> root user

Change user:
FROM ubuntu
USER repl
RUN apt-get update
```

### Variables in Dockerfile
```Dockerfile
ARG <var_name>=<var_value>
ARG path=/home/repl

# To use in Dockerfile
COPY /local/path $path

# Example 1
FROM ubuntu
ARG python_version=3.9.7-1+bionic1
RUN apt-get install python3
RUN apt-get install python3-dev=$python_version

# Example 2
FROM ubuntu
ARG project_folder=/projects/pipeline_v3
COPY /local/project/files $project_folder
COPY /local/project/test_files $project_folder/tests
```

### Setting a variable in the build command
```bash
docker build --build-arg project_folder=/projects/pipeline_v3 .
```

### Variables with ENV
```Dockerfile
ENV <var_name>=<var_value>
ENV DB_USER=pipeline_user

CMD psql -u $DB_USER

# Setting a directory to be used at runtime
ENV DATA_DIR=/usr/local/var/postgres
ENV MODE production
```

### Setting environment variables in the run command
```bash
docker run --env <key>=<value> <image-name>
docker run --env POSTGRES_USER=test_db --env POSTGRES_PASSWORD=1234 postgres
```

```
Variables are not secure because they are stored in docker history
```

### Creating secure docker images
```
- Attackers can exceptionally break out of a container.
- Start with an image from a trusted source => Docker Hub filters 
- Keep software up-to-date
```

### Docker help
```bash
docker run --help
```

### Temporary containers
```
- Docker containers are usually created with docker run 
- Containers remain even after stopping / existing
- Often want to run a container instance and remove it immediately upon exit
    - Development
    - Testing
    - Scripts
```

### Running containers temporarily
```bash
docker run --rm <image-name> # remove container after exit
docker run --rm alpine:latest /bin/sh  
```

### Mounting the host filesystem
```
Container filesystem:
- Container instances each have their own filesystem
   - Based off the image the container was created with 
- Any changes are tied to that spesific container instance
- Any changes are maintained accross restarts
    - For that instance only
- New containers only have the data in the image, not instance specific changes

Sharing files or directories:
- Can attach specific files or directories to containers
- Allow for persistence of data, without maintaining a specific container.
- Can upgrade container to new version but safely keep data / change.
- Known as bind-mount.
- Can be read-only or read/write.

Using the - v option 
- bind-mount most often use the -v flag.
- -v <source>:<destination>
- Multiple -v commands permitted.
- Can also use the --mount option.
```

### Bind-mounting
```bash
docker run -v ~/html:/var/www/html ngix

# ~/html        -> source (host)
# /var/www/html -> destination (container)
# ngix          -> image
```

### Run a container with a bind-mount
```bash
docker run 
    -v ~/pgdata:/opt/data \
    -v ~/pg.conf:etc/pg.conf 
    postgresql
```

### Persistent volumes
```
Volumes:
- An option to store data in Docker, unrelated to the container image or host filesystem
- Are managed from the command line 
- Can share with multiple containers
- Higher performance that file share / bind mounts
- Exist until removed
```

### Managing volumes
```bash
docker volume ls                        # list volumes
docker volume                           # manage volumes
docker volume create <volume-name>      # create a volume
docker volume inspect <volume-name>     # get details
docker volume rm <volume-name>          # remove a volume
docker volume rm $(docker volume ls -q) # remove all volumes

docker volume create sqldata
docker volume inspect sqldata
```

### Attaching volumes to containers:
```bash
# Use the -v flag
docker run -v <volume-name>:<container-destination> <image-name>
docker run -v sqldata:/var/lib/postgresql/data postgres
```

Condition | 	Use
--- | ---
Want to access data from a local folder?	| Bind Mount (./postgres_data:/var/lib/postgresql/data)
Want Docker to handle data without worrying about permissions?	| Named Volume (postgres_data:/var/lib/postgresql/data)
Deploying to a server (more secure)?	| Named Volume

### Drivers
```
- Methods of storing Docker volumes
- Can include:
    - Local filesystem (default)
    - NFS (Network File System)
    - SMB / CIFS (Server Message Block / Common Internet File System)
    - Cloud storage (AWS, Azure, Google Cloud)
```

### Networking in Docker
```
What is networking?
- A computer network consists of systems communicating via a defined method.
- Varying levels of communication, physical or logical, refeerred to as protocols
- Common physical networks include Ethernet and WiFi
- Logical networking includes TCP/IP, HTTP, SMTP

Networking terms:
- Host        -> General term for a computer
- Network     -> Group of hosts
- Interface   -> Actual connection from a host to a network, such as Ethernet or WiFi.
- LAN         -> Local Area Network, or set  of computers at a single location
- VLAN        -> Virtual LAN, or a logical grouping of computers
```

```
Internet Protocol:
- IP    -> Internet Protocol, method to connect between networks using IP address
- IPv4  -> Versio of IP supporting 4.2 billion addresses, currently exhausted.
- IPv6  -> Version of IP supporting 340 undecillion addresses, currently in use and being deployed.

IPv4: 10.10.10.1
IPv6: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

```
TCP/UDP
- TCP -> Transmission Control Protocol, reliable connection-based protocol
- UDP -> User Datagram Protocol, connectionless protocol
```

```
Ports:
- Port -> Addresses services on a given host, a value between 0 and 65535, used to communicate between hosts via TCP or UDP
    Ports below 1024 are typically reserved for priviledge accounts
    Ports above 1024 are usually ephemeral or temporary ports
    Application listen on a port

Application protocols
- HTTP/HTTPS  -> Application protocol, defaulting to TCP port 80 for web communication. Secure version on TCP port 443.
- SMTP        -> Simple Mail Transfer Protocol, used for email communication on TCP port 25.
- SNMP        -> Simple Network Management Protocol, used for network management on UDP port 161.
```

```
Docker and networking:
- Can communicate between containers
- Can comunicate with the host system
- Depending on settings can communicate with external hosts
- Typical communication is handled by exposing ports from the container to the host

Docker and IP
- Containers can have IP addresses
- Use ifconfig <interface> or ip addr show <interface> to see IP addresses
- Use ping -c <x> <host> to test connectivity
  ping -c 3 google.com
```

### Making network services available in Docker
```
Network services:
- Network services listen on a given port
- Only one program can listen on an IP:port combo at a given time.
    - For example, 10.1.2.3:80 would be listening on 10.1.2.3 on port 80
- Consider trying to debug different versions of a web server that listens on port 80
    - Could only run one copy of the application at a time given that it listens only on that port

Containerized services:
- Wrapping application in a container means thatt each container can now listen on that port 
- Can have multiple copies of the containers running at once
- But how too connect to container's version of application from the host?
```

### Port mapping
```
- Port mapping takes a connection to a given ip:port and automatically forwards it to another ip:port combo
- In this case, we could map an unused port on our host and point it to port 80 on the container

Enabling port mapping
- TO enable port mapping on a given container, we use the docker run command, and the -p flag
-p <host port>:<container port>
-p 5501:80
```

### Docker container port mapping
```bash
docker run -p 5501:80 nginx
```

### Ports with Dockerfiles
```
Exposing services:
- EXPOSE command
- Defines which ports the container will use at runtime
- Can be defined as <number> or <number>/<tcp> or <number>/<udp>
    - EXPOSE 80, EXPOSE 80/tcp, EXPOSE 80/udp
- Multiple entries permitted
```

```Dockerfile
FROM python:3.11-slim
ENTRYPOINT ["python", "-mhttp-server"]
# Let the docker engine know
# port 8000 should be exposed
EXPOSE 8000
```

### Check docker container port mapping
```bash
docker run pyserver
docker run -P pyserver # make the port reachable
docker inspect <container-id> # get the port mapping
```

### Docker networking
```
- Docker has extensive networking options
- Can create networks to communicate between containers, host, and external systems

Docker networking types:
- Docker support different networking types, using drivers.
    - Bridge -> Default network, private to the host
    - Host -> Allows full communication between host and container
    - none -> Isolate container from network
```

### Manage networks
```bash
docker network                        # manage networks
docker network create <network-name>  # create a network
docker network ls                     # list networks
docker network inspect <network-name> # get details
docker network rm <network-name>      # remove a network
docker network prune -f               # remove all unused networks
```

### Attaching containers to networks
```bash
docker run --network <network-name> <image-name>
docker network connect <network-name> <container-name>
```

### Building advanced container images
```
Optimizing Docker images
- Docker images can be large
- Large images can take longer to build, push, and pull
- Large images can consume more disk space
- Large images can be slower to start

Docker image recommendations:
- Split large images into smaller images
- Use multi-stage builds
- Use .dockerignore to exclude files
- Use smaller base images

Example with minimized containers:
- Better options with Docker
- Split each into its own container.
    - Postgresql database container
    - Python ETL container
    - Web server container
```

### Docker layers
```
- Docker images are made up of layers
- Layer generally references a change or command within a Dockerfile
- Layers can be cached / reused

FROM ubuntu                 -> layer 1
RUN apt-get update          -> layer 2
RUN apt-get install python3 -> layer 3
```

## Docker layers inspection
```bash
docker image inspect <img id>                 # get the image details
docker image inspect <id> | jq '.[0].RootFS'  # get the image 
docker image inspect <id> | jq '.[0] | {layerCount: .RootFS.Layers | length}' # get the layer count
```

### Multi-stage builds
```
- Multi-stage builds allow for multiple FROM statements in a single Dockerfile
- Each FROM statement starts a new build stage
- Each stage can copy files from previous stages

AS <alias>          -> set an alias for a stage
COPY --from=<alias> -> copy files from a previous stage
```

```Dockerfile
FROM ubuntu AS stage1
RUN apt-get update
RUN apt-get install python3
RUN make

# Start new stage to create final image
FROM alpine-base
COPY --from=stage1 /data/app /data/app
CMD ["python3", "/data/app/app.py"]
```

### Multi-stage builds in Dockerfile
```Dockerfile
FROM golang:1.21-alpine3.19 AS gobuild

WORKDIR /src
COPY /src/main.go /src/main.go

# Build the go application from source code
RUN go build -o /bin/app_runner /src/main.go

# Create application in standalone container
# Update with the image name from instructions

FROM scratch

# Update with name of previous build stage
COPY --from=gobuild /bin/app_runner /bin/app_runner

CMD ["/bin/app_runner"]
```

```
--platform=$BUILDPLATFORM flag  -> specify the platform
--platform=linux/amd64          -> specify the platform
- use ARG to specify the platform 
- TARGETOS and TARGETARCH       -> specify the platform
```

```Dockerfile
FROM --platform=$BUILDPLATFORM golang:1.21-alpine3.19 AS gobuild
WORKDIR /src
COPY /src/main.go /src/main.go
ARG TARGETOS TARGETARCH
RUN env=GOOS=$TARGETOS GOARCH=$TARGETARCH go build -o /bin/app_runner /src/main.go
```

### Build multi-platform images
```bash
docker buildx create --name mybuilder # create a builder
docker buildx use mybuilder 

docker buildx build --platform linux/amd64, linux/arm64 -t myapp .
docker buildx create --bootstrap --use -> create and use a builder
```

### Multi-stage builds in Dockerfile example
```Dockerfile
FROM line to support a multi-platform build
FROM --platform=$BUILDPLATFORM golang:1.21-alpine3.19 AS gobuild

WORKDIR /src
COPY src/main.go /src/main.go

# Define the option to pull environment variables from the host
ARG TARGETOS TARGETARCH

# Build the go application from source code
RUN env GOOS=$TARGETOS GOARCH=$TARGETARCH go build -o /bin/app_runner /src/main.go

# Create application in standalone container
# Update with the image name from instructions

FROM scratch

# Update with name of previous build stage
COPY --from=gobuild /bin/app_runner /bin/app_runner

CMD ["/bin/app_runner"]
```

### Multi-stage builds in Dockerfile using docker-compose
```
- Specify containers, neworking, and storage volumes in a single file
- docker-compose.yaml

# Define the services
services:
    # Define the container(s), by name
    webapp:
        image: "webapp"
        ports: 
            - "8000:5000"
    redis: 
        image: "redis:alpine"
```

## Build multi-platform images from docker-compose
```bash
docker compose up             # start the services
docker compose up -d --build  # rebuild the services
docker compose down           # stop the services
docker compose -f <file> up   # specify the file
docker compose up -d          # run in detached mode
docker compose ls             # list the services
```

### Docker-compose
```
- Yet Another Markup Language
- Human-readable data serialization standard
- Text file, but spacing is important

Main sections:
- services  -> list the containers to load
- networks  -> define the network
- volumes   -> define the volumes
- configs   -> define the configs
- secrets   -> define the secrets (pass, token, api)

Services section:
- container_name  -> name of the container
- image           -> image to use
- build           -> build the image
- ports           -> expose ports
- volumes         -> mount volumes
- networks        -> attach to networks
```

### Docker-compose file format
```yaml
  # Specify the resource name
  primary:
    
    # Define a container name
    container_name: webapp
    
    # Use the proper image
    image: test_app:latest
    
    # Define the correct entries to forward ports
    ports:
      - "8000:8000"
```

### Docker-compose file example
```yaml
  redis:
    image: redislabs/redismod
    ports:
      - '6379:6379'
  flask:
    image: flaskapp
    ports:
      - '8000:8000'
    depends_on:
      - redis
  nginx:
    image: nginx
    ports:
      - '80:80'

```

```
- Resources may depend on other resources
- Web app:
    - Database container postgresql must start first 
    - Then the python_app

condition: 
- service_started -> wait for the service to start
- service_completed_successfully -> wait for the service to complete
- service_healthy -> wait for the service to be healthy
```

### Check status of services
```bash
docker compose logs <service-name>  # get the logs
docker compose ps                   # get the status
docker compose top                   # get the top
```

### Docker-compose file with condition
```yaml
  web:
    image: custom_app
    depends_on:
      - redis
  llm:
    image: openai-llm:14
    depends_on:
      - web
  forwarder:
    image: fwd_proxy/latest
  redis:
    image: redis:latest
    depends_on:
      forwarder:
        condition: service_healthy
```

### Docker-compose file with networks
```yaml
  dataservice:
    image: dataservice
    volumes:
      - /home/repl/datasets:/data
    networks: 
      data_net:
        ipv4_address: 10.11.12.2

  dataclient:
    image: dataclient
    depends_on: 
      - dataservice
    networks:
      data_net:
        ipv4_address: 10.11.12.3

# Ignore everything below this line
networks:
  data_net:
    driver: macvlan
    ipam:
      config:
        - subnet: "10.11.12.0/24"
```    

<br>

---
### NOTES
---
### Run jupyter notebook in docker
```bash
docker run -p 8890:8888 -v $(pwd):/home/jovyan/work jupyter/base-notebook
```

```bash
sudo chmod -R 777 pgadmin_data # change permission full access (read, write, execute)
```

### Command to run a container manually
```bash
docker run -it \
    --network=project1-network \
    --name ny-taxi-etl-container \
    ny-taxi-etl:v01 \
        --user=admin \
        --password=admin123 \
        --host=postgres_db \
        --port=5432 \
        --db=ny_taxi \
        --table_name=yellow_taxi_trips_dec2024 \
        --url="https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-12.parquet"
```

### Command to access the container
```bash
docker exec -it <CONTAINER_ID> python app.py \
    --user=admin \
    --password=admin123 \
    --host=postgres_db \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips_jan2024 \
    --url="https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet"
```

### Using requirements.txt to add packages to the already built image and container
```Dockerfile
FROM apache/airflow:2.10.5-python3.10

USER root

RUN apt-get update && apt-get install -y

USER airflow

RUN pip install --upgrade pip

# Copy requirements.txt to the container
COPY requirements.txt /requirements.txt

# Install dependencies
RUN pip install --no-cache-dir -r /requirements.txt
```

```bash
docker build -t your-image-name .
docker build -t airflow:v02 .
docker run -d --name your-container-name airflow:v02
```

### Install new package to the already built image and container
```bash
# Copy file requirements.txt to the container
docker cp path/to/requirements.txt your-container-name:/path/in/container
docker cp /tmp/requirements.txt airflow:/tmp/requirements.txt

# Access the container 
docker exec -it your-container-name bash

# Install the new package
pip install --no-cache-dir -r /tmp/requirements.txt

# Commit the changes
docker commit your-container-name your-new-image-name
docker commit your-container-name airflow:v02
```
