# Simple generator for Kubernetes resources

If you used the starter service, then the generator has already been executed and you can build and run the application and provision it's dependent services.


### Deploy required services to Kubernetes

If using `mysql` service, create a secret

```
kubectl create secret generic mysql \
  --from-literal=mysql-root-password=$(echo $RANDOM) \
  --from-literal=mysql-password=$(echo $RANDOM)
```

If you recreate the secret and have already run the `mysql` service, also delete the persistent-volume in addition to the secret

```
kubectl delete secret mysql
kubectl delete pvc mysql
```

Now deploy the services:

```
kubectl apply -f kubernetes/services/**
```

## Building and running the application

This simple generator depends on a configuration to comtainerize your project code. The easiest way to do this is using the new `buildpack` support added in Spring Boot version 2.3.

### Build the Spring Boot project

If you are using Minikube you should configure your terminal to use the same Docker environment:

```bash
eval $(minikube docker-env)
```

Now, build the project and the project and create the container image:

```
./mvnw clean package spring-boot:build-image
```

### Deploy Application to Kubernetes

```
kubectl apply -f kubernetes/app
```

Use `kubectl get all` to verify that the resources created.

Accessing the application's endpoint varies based on the type of Kubernetes cluster you are using.  For example, if minikube is being used, look for the endpoint to access using the command `minikube service list`

The README.md file from the code repository for the appliation should include a few `curl` commands to exercise the application.



# Generator Information

This generator creates the Kubernetes `service` and `deploymnet` resources files to deploy a Spring Boot application using `kubectl`

You will need to install the following command line tool:

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)


## Generator Commands

`k8s-simple new` creates

* A `kubernetes` directory with a `service.yaml` and `deployment.yaml`

`k8s-simple new-services` creates

* A `kubernetes/services/<service-name>` directory with Kubernetes resource files for various services.  Currently only `mysql` service name is supported.


## Generator installation

```
tss generator install --go-getter-url=github.com/markpollack/generator-k8s-simple
```

To use the install command you need to install [go-getter](https://github.com/hashicorp/go-getter#installation-and-usage) CLI.
