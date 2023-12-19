# Two-Tier-Flask-App-Deployment-Using-HELM  - PART-3

                             TWO-TIER-ARCHITECTURE-FOR-HELM


  Process deployment using in helm using kubernetes cluster


![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/394954ad-80c3-4900-bb74-857cc8ba686d)



HELM :-

* So when we try to use kubernetes we required deployment.yaml /service / secrets /ingress YAML
we required number of YAML manifest to run a application 
* Diffcult to keep the number of YAML manifest in any location are directory and diffcult to maitain as well
* So we required HELM is a package manager for kubernetes file can be managed and it help to packaging
* By using the package we can install / upgrade and uninstall can be done.
* As a package you can run any where it will be like single entity
* By using HELM we can build template

  Section 1: Introduction and Helm Basics
What is Helm?
Helm is often referred to as the package manager for Kubernetes. It enables you to define, install, and manage even the most complex Kubernetes applications. Helm uses a packaging format called charts, which include all the resources needed to run an application, service, or a complete cloud-native stack inside Kubernetes.

How to Install helm in Ubuntu ⤵️

curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm


Important Helm Commands
    • helm create [CHART]: Scaffold a new Helm chart. 
    • helm package [CHART]: Package the chart into a chart archive. 
    • helm install [NAME] [CHART]: Install a Helm chart. 
    • helm upgrade [NAME] [CHART]: Upgrade an installed Helm chart. 
    • helm uninstall [NAME]: Uninstall an installed Helm chart. 
    • helm list: List all installed Helm charts. 
    • helm rollback [NAME] [REVISION]: Roll back a release to a specific revision. 
Section 2: Prerequisites
    • Helm installed 
    • Kubernetes cluster set up (e.g., Minikube, kind, or any cloud-based Kubernetes) 
    • Docker installed (Optional for custom images) 
    • Basic understanding of Kubernetes resources like Pod, Service, Deployment 


Step 1 :- create a helm two-tier-app for mysql
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/66a4f533-9bd0-48f6-b3e2-5f3c6de7fafe)



Step 2:-  cd to mysql chart 
check the cat values.yml or vi values.yml

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/00356b14-f499-41cd-b989-8819e493c1fa)


 Values.yaml -----> you can default YAML structure has bee defined with nginx 
    
Below screen-shot you see form Values.yaml (before we are creating Yaml manifest but Helm will give chart to create will default YAML format will be avaiable with required YAML manifest file
Just required to add and change in requried fields

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/4e797e20-efae-4bb1-b31c-0e95564973c4)




next 

vim values.yaml

Changed some values (if required you can change replica count how required for )

for practise I mentioned only 1 in replicaCount :1
next :- tag: “latest”
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/d01c9e35-3680-4695-bf2d-7a4ad98b747b)

Changed port : 3306 because mysql I have given in manifest to run in 3306 changing the port number
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/24182fd5-7776-45e3-89b4-a96187fb2ece)

By changing only with we can able to run mysql container we required Environment variables as well

Please check once in Mysql-deployment.yml  file variables we already given copy from Yml file

Before we have add values in values.yaml

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/66c454be-1f8b-4ca4-b898-9cdf1ba34012)

You can see on above screenshot enviroment variable has been provided in values.yaml
 why beacuse if i required to change anuthying in DB it pass the value form deployment.YAML
how next you can see how it will pass 

Next :- add variable to the values from deployment.YAML file
Above you can see env i have given {{ ....}}
To pass the value in to deloyment.yaml if change value in values.yaml file in environment 
  Value will passed from deployment.YAML
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/f90c3203-5a2a-4b9a-9b01-0427e609ffd8)

Up to we have edited the template we required 
Next required change the liveprobe and redinessProbe not required it means (helm check api’s)
Not required because MySQL is a database server 

So # commets to untag it ---> go to deployment.YAML # to untag 
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/bac0882f-44ef-4600-af9b-f281804adbb8)

NEXT 

After done the changes package the mysql-chart

CMD :-  helm package mysql-chart
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/0660713f-47b0-4c1a-8e2b-c9eed0b3d9be)



Next :- 

After package mysql-chart and next step to install the package 
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/cb70346f-39a4-4ca5-8064-7aeb19ea78fe)



In case display any error it show in where the error in whether in deployment or values and other shiow with lines 

You can go to the YAML file resolve it max indentation or value may be incorrect

next required to uninstall the mysql package 

Package again mysql-chart 

Next :- helm install mysql-chart ./mysql-chart

Now result will show DEPLOYED ---SUCCESS

CMD :- kubectl get all

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/fc0fb816-deec-4cf8-b46b-497a9259ceb6)



POD is running    1/1
Service is running  with port 
deployment created 
replicaset created

NEXT :- All are working in mysql   --> to conform --> go to worker node 

CMD :- sudo docker ps

you can see my sql container is running

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/b7d2b9ad-1a5b-4781-ae38-72ebfc3fe80e)


NEXT :- LOGIN TO MYSQL CONTAINER 
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/6c83e77f-7d57-43f3-8a10-1ae3e3e7655a)



Successfully login-In to MYSQL – CONTAINER

NOW success on created  MYSQL – DATABASE    Container 

Now required to create backend -Container of flask api 


NEXT :-

CMD : helm create flask-app-chart

Node need create a YAML manifest HELM give a default template to add values in attribute an pass the value

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/ac76a99a-bfb9-40e0-b0f7-ff89e2b34039)


Next changes required in Values.yaml 
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/d165083d-1829-4dad-810e-42ae3207ff25)


Next :- change in service drag down to service
![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/b87b3127-74d6-49dc-b32f-6c782bdb6da6)











Chnage Type :- NodePort
Port is : 80 
targetPort : 5000    -all pod will tagged with port 5000 added with port 80
Nodeport: 30007     -last it tagged with 30007 to access from outside of node 


NEXT :- 

Go to deployment.Yaml 

give environemt variables to pass the values 

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/a2d21f36-1443-4cc6-951d-8ea48b26a454)

Next change below on target port 
    Before it will be {{ .Values.service.port }}
        We have to change to {{ .Values.service.targetport }}

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/166a8034-4c9d-41ae-8bb6-468b7fbcd99f)

Next go to service.yaml because target port mention to pass the values

Service.yaml

HERE WE ARE EXPOSING THE APPLICATION REQUIRED TO GET VALUES 

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/87000066-9a92-4d2f-ad26-cabcf60272f0)


Done changes 

port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.targertPort }}
      nodePort: {{ .Values.service.nodePort }}


NEXT :- Before package and run check given all values and port are correct

CMD :-  helm template flask-app-chart


![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/dcb81164-fc48-4043-98fe-b3fa20cde360)


ERROR :- 

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/337e09be-d158-461c-9ffa-ee5e4628c65b)




Erro found in deployment.yaml file  I have provided targetport but it has to be targetPort 

Port ---P is capital if i give small it can able to match the values with values.yaml file

Again packaged and installed 

![image](https://github.com/DineshA055/Two-Tier-Flask-App-Deployment-Using-HELM/assets/101075223/f4ce77d4-5e0b-498d-ab74-6ea00d4f46e2)

SUCCESS On CREATING FLASK APP

Kubectl get all 

To check all pod are running
Service also running

Next :- Check port number and Ip is same validate once 

CMD :- helm template flask-app-chart 


Gives list of all item to validate

Now Port is running you can see in terminal 

Cluster Instance IP wiith  service port expose for flask-app
Make sure port is open 

Now application running successfully in browser 




********** END of HELM ************** If any update will continue update in readme*************************************
