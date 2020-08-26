# Simple generator for Kubernetes resources

This generator creates the Kubernetes `service` and `deploymnet` resources files to deploy a Spring Boot application using `kubectl`

You will need to install the following command line tool:

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)


## Generator Commands

`k8s-simple new` creates

* A `kubernetes` directory with a `service.yaml` and `deployment.yaml`


## Generator installation

```
tss generator install --go-getter-url=github.com/markpollack/generator-k8s-simple
```

To use the install command you need to install [go-getter](https://github.com/hashicorp/go-getter#installation-and-usage) CLI.

## Building and running the application

This simple generator depends on a configyration to comtainerize your project code. The easiest way to do this is using the new `buildpack` support added in Spring Boot version 2.3.

### Build the Spring Boot project

If you are using Minikube you should configure your terminal to use the same Docker environment:

```bash
eval $(minikube docker-env)
```

Now, build the project and the project and create the container image:

```
./mvnw clean package spring-boot:build-image
```

### Deploy to Kubernetes

```
kubectl apply -f kubernetes/
```

Use `kubectl get all` to verify that the resources created.

Accessing the application's endpoint varies based on the type of Kubernetes cluster you are using.  For example, if minikube is being used, look for the endpoint to access using the command `minikube service list`
