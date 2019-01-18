# Deployment of a java web application onto Kubernetes
## Preperation 
### Deployed Kubernetes and docker environment  
We have done this in Task1.
### Prepared web application 
We have done this in Task2. 
### A ready mirror image for our web application  
After all the above, type:
```
docker build -t /rrproj/EatOrNot
```
And we've got our web application packed into a mirror image and ready to deploy on K8s.
Note: In this part, we packed both frontend and backend into a mirror image for simplicity.  
## Deployment
The key step for this task is to config the deployment.yaml.
Since we use local mirror image to deploy, we have to modify the deployment.yaml as following:  
![Img](pics/yaml.jpg)
## Visit our web application
By visiting localhost:
## For more details, see the video in this folder.
