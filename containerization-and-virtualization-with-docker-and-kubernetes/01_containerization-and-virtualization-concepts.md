## [Containerization and Virtualization Concepts](https://app.datacamp.com/learn/courses/containerization-and-virtualization-concepts)

```
Personal computers in our daily lives
Servers enabling business applications
```

### Virtual machines
```
- A simulated computer system within another computer system
- Each VM operates independently
```

### Virtualization
```
- Virtual machines (VMs) are often discussed as a solution to the limitations of physical machines, providing a more flexible and efficient way to use computing resources
- Process of creating a virtual of a computer resource
- Full virtualization:
    - Virtualizing the entire computer system
    - Results in VM
```

### Containerization
```
-> Virtualization at the operating system level

OS-level virtualization:
- Virtualizing the operating system
- Not virtualized: hardware, kernel, and other resources
- Virtualized: isolated user spaces, file systems, and network interfaces

OS-level virtualization     = Containerization
Isolated user spaces        = Container
```

### Containers
```
- The process of packaging an application and its dependencies into a container, allowing it to be deployed and executed consistently accross different environments.
- Isolated environment
- Includes application and all dependencies

application -> container
```

### Characteristics when using containers
```
- Reliably running multiple applications on a single host
- Each application in its own container
- Overview of application dependencies

Benefits of containers:
- Isolation between applications
- Portability & reproducibility
- Fast startup times
```

### Virtualization vs. Containerization
```
- Virtualization:
    - Full virtualization
    - Virtualizing the entire computer system
    - VMs

- Containerization:
    - OS-level virtualization
    - Virtualizing the operating system
    - Containers: isolated app environtment

Container management    : docker
Container orchestration : Kubernetes

Virtualization: 
- Oracle VM VirtualBox
- VMware
```

### Containerization with Docker
```
- Managing the lifecycle of containers

Most important Docker components:
- Docker Desktop    -> GUI
- Docker Engine     -> CLI (Command Line Interface)
    - Docker client
    - Docker daemon (server)
- Docker Objects
    - Docker Images -> blueprint
    - Docker Containers -> running instance of an image
- Docker Registries -> store and distribute Docker Images
    - Docker Hub
    - Docker Store
    - Docker Cloud

Dockerfile (build) -> Docker Image (run) -> Docker Container
```

### Orchestration: The automated management of multiple components
```
Declarative programming in container orchestration:
- Describes the desired state of the system
config file -> orchestrator -> setup

Application of container orchestration
- Microservices architecture
- Application scaling
- Automation of pipelines

Container orchestration tools:
- Kubernetes
- Docker Swarm
- OpenShift
- Nomad by HashiCorp
```

### Container orchestration with Kubernetes
```
Abbv: K8s
- Grouping containers into logical units
- Distributed system

Most important Kubernetes components:
- Pods -> smallest unit in K8s, one or more containers
- Nodes -> physical or virtual machine
- Control Plane -> manages the cluster
- Cluster -> group of nodes

Docker      : one or few containers
Kubernetes  : many containers
```

### Reading Dockerfile and running a container
```
Sequential order in Dockerfiles:
- Execution in sequential order
- Start of a Dockerfile:
    - Metadata
    - Comments
    - Arguments
    - FROM instruction 

Important Docker instructions:
- FROM
- COPY
- RUN 
- ENTRYPOINT

FROM:
- Specifies an existing Docker image
- Defines the image we are building on 
    - Starting point
- Syntax:
    FROM <name_of_image>:<tag>
- Example:
    FROM python:3.10

COPY:
- Copies files or directories
- From source to destination
- Files that are needed in following Docker instructions
- Syntax:
    COPY <source> <destination>
- Example:
    COPY . /app

RUN:
- Runs a command in the container
- Syntax:
    RUN <command>
- Example:
    RUN pip install -r requirements.txt

ENTRYPOINT:
- Defines the container's default behavior
- Specifies command to run at initiation
- The primary purpose of the container 
- Syntax:
    ENTRYPOINT ["command", "argument']
- Example:
    ENTRYPOINT ["python", "app.py"]
```

### Dockerfile example
```bash
FROM python:3.10

COPY . .

RUN pip install -r requirements.txt

ENTRYPOINT ["python", "app.py"]
```

### Docker build command
```
- Docker needs to be located in build context
- Executed as command with Docker client via CLI
- Syntax:
    docker build context
```

### Docker run command
```
- Docker image needs to be specified as argument 
- Executed as command with Docker client via CLI
- Syntax:
    docker run <image_name>
```