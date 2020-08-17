# Simple generator for Kubernetes resources

This generator creates a Dockerfile to containerize a Spring Boot application and well as the Kubernetes `service` and `deploymnet` resources files to deploy a Spring Boot application using `kubectl`

You will need to install the following command line tool:

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)


## Generator Commands

`k8s-simple new` creates

* A `Dockerfile` in the root directory
* A `kubernetes` directory with a `service.yaml` and `deployment.yaml`


## Generator installation

```
tss generator install --go-getter-url=github.com/markpollack/generator-k8s-simple
```

To use the install command you need to install [go-getter](https://github.com/hashicorp/go-getter#installation-and-usage) CLI.

## Building and running the application

There are some assumptions baked into this simple generator.  The generated Dockerfile has the main class name of `com.example.demo.DemoApplication`.

These are the default main class when creating a Spring Boot application from (start.spring.io) using the following `curl` command.

```
curl https://start.spring.io/starter.tgz -d dependencies=web,actuator -d javaVersion=1.8 | tar -xzvf -
```

Please change the Dockerfile's main class as appropriate.  Note, more advanced generators, ones that build using the Jib plugin to Buildpacks,  would not require you to make this type of change.

### Build the Spring Boot uber-jar

```
./mvn clean package
```

### Create the container
```
docker build -t demo:0.0.1-SNAPSHOT
```

Note that `0.0.1.SNAPSHOT` should match the `<artifactId/>` in `pom.xml`


### Deploy to Kubernetes

```
kubectl apply -f kubernetes/
```

Use `kubectl get all` to verify that the resources created.

Accessing the application's endpoint varies based on the type of Kubernetes cluster you are using.  For example, if minikube is being used, look for the endpoint to access using the command `minikube service list`

