# aspnetcore-minikube-example

Tutorial for basic asp.net core web api deployed on kubernetes via minikube and docker.

## Useful Links
* https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-vsc
* https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/
* https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-in-kubernetes/42564211

## Versions
* minikube version: v0.24.1
* docker version 17.12.0-ce, build c97c6d6
* dotnet version 2.1.4
* macOS version 10.13.2

## Requirements
* ASP.NET Core SDK
* Minikube
* Docker
* Visual Studio Code C# extension (optional)

## Setup

1. Setup Project 
``` 
mkdir GradeService
cd GradeService
dotnet new webapi
```
2. Add dockerfile which exposes port 8080 at GradeService root.
3. Don't need to make any additional changes beyond this point. We will use the autogenerated ValuesController for testing.

## Building Docker Image

We will be using minikube's local docker deamon. Docker images will not be hosted remotely. https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-in-kubernetes/42564211

1. ```eval $(minikube docker-env)```
2. ``` docker build -t grade-service:v1 .```
    * If you do not demarcate your image tag version it will be given the tag of latest. Kubectl will attempt to remotely pull images tagged with latest, and we do not want this.
3. Save kubernetes cluster master ip. ```minikube cluster info``` 
4. Run docker container to quickly test. ```docker run -it --rm -p 8080:8080 --name grade-service grade-service:v1```
5. In web browser or postman navigate to ```<step 3 ip>:8080/api/values```

## Kubernetes Deployment w/ Docker images
1. ```kubectl run grade-service --image=grade-service:v1 --port=8080 imagePullPolicy=Never```
2. Check deployment and pod ```kubectl get deployments``` and ```kubectl get pods``` 
3. Create and expose service ```kubectl expose deployment grade-service --type=LoadBalancer```
4. Check services ```kubectl get services```
5. Navigate to service ```minikube service grade-service``` and append ```/api/values```

## Next Steps
You should have your asp.net core web api running on a kubernetes cluster via minikube and docker. Now would be a good time to expand the web api, play around with multiple services and docker-compose, explore kubectl yaml config files (you can explore the default ones created for you during this tutorial via `minikube dashboard`).