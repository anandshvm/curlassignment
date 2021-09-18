# Summary

This project has simple golang http server, that can be deployed on kubernetes using deployments file or helm .

main.go has simple rest endpoint /hello, which returns the ENV variables

# Dockerfile

1. Its a multi-stage build using golang:alpine image in first stage to build the golang executable
2. In second stage we use only scratch image to run the build created in previous step


Steps:

```
docker build -t test .
docker run -d -p 31000:31000 test 
docker push ashivam/myassignment:latest


```
 
 
# Deployment
This project can be deployed using the deployment files present inisde deployments folder. It has deployment and service files
Application is deployed as 2 pod replicas and is exposed using service at port 31000 to access te application we can port forward accordingly. 

Steps: 
        
```
kubectl apply -f deployments/
kubectl port-forward svc/microsvc 31000:31000 

```
         
This will expose localhost:31000/hello endpoint, which returns env variables 

SENDER_EMAIL=myemail@gmail.com
PASSWORD=myPassword
RECIEVER_EMAIL=myreciever@gmail.com
   
   
# Deployment with helm 

This app can be deployed using the helm. All helm files are present inside testcharts. It has values.yml file where we can set all the variables that are referred inside the deployment and services. We access the values for env inside the contianer section of deployment from values.yml. values.yml can be overridden by command line while installing helm chart.
  
Steps: 

```
helm create testcharts
helm install myhelm . --set config.SENDER_EMAIL=myemail@gmail.com --set config.PASSWORD=myPass --set config.RECIEVER_EMAIL=myreciever@gmail.com

kubectl get pods 

kubectl get services 

kubectl port-forward svc/microsvc 31000:31000 

```
  
  
  
