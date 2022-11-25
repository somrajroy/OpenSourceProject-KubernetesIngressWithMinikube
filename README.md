# Deploying applications using Ingress - Minikube <br/>
Deploying applications using Ingress using minikube. Good for quick testing, POC's, Demo etc purposes <br/>
* Below is example where an Ingress sends all its traffic to one Service. In this demo we will use ingress to send traffic to 3 services - cats, dogs and birds:<br/>
  ![image](https://user-images.githubusercontent.com/92582005/202117909-defb991d-8f12-49e5-ad9b-91f3f169399c.png) <br/>
* Ingress controllers work at layer 7 and can use more intelligent rules to distribute application traffic. Ingress controllers typically route HTTP/HTTPS traffic to different applications based on the inbound URL.<br/>
    ![image](https://user-images.githubusercontent.com/92582005/203915170-8a64780b-2c6f-4a50-b91c-c6e04fcd1e05.png)<br/>
### Steps to be followed : <br/>
* Clone the repository and navigate to lab-05 folder in Powershell/CLI <br/>
* Enable the NGINX Ingress controller in minikube <br/>
  $ minikube addons enable ingress <br/>
* Below success message will come <br/>
  ![image](https://user-images.githubusercontent.com/92582005/201918233-c7415fb3-91fe-4a40-8387-c6c2411231e1.png) <br/>
* A new namespace "ingress-nginx" will be created. Inspect the pods and services <br/>
  $ kubectl get svc -n ingress-nginx <br/>
  $ kubectl get pods -n ingress-nginx <br/>
  ![image](https://user-images.githubusercontent.com/92582005/201918899-73275eb7-761c-45c1-8488-96fbb62778dd.png) <br/> 
  ![image](https://user-images.githubusercontent.com/92582005/201918986-f9bf31f3-7509-46ae-8bd2-7dd830b5a351.png) <br/>
* Deploy the NGINX ingress controller in minikube (**Copy/Paste of below command from Github sometimes gives error. In that case clean up the enviornment as mentioned in last section and then run the command from the "command.txt" file uploaded in the repo(enabling ingress addon (NGINX Ingress controller) once again from the step mentioned in first step)**) <br/>
 New command to create NGINX ingress controller without customizing the defaults<br/>
 $ helm install nginx-ingress ingress-nginx/ingress-nginx --create-namespace --namespace ingress-basic --set controller.replicaCount=2 --set controller.nodeSelector."kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux <br/>
 Old command (deprecated but still may work) <br/>
 $ helm install nginx-ingress ingress-nginx/ingress-nginx --create-namespace --namespace ingress-basic --set controller.replicaCount=2 --set controller.nodeSelector."beta.kubernetes.io/os"=linux --set defaultBackend.nodeSelector."beta.kubernetes.io/os"=linux <br/>
### Deploy  <br/>
* Deploy cats, dogs and birds applications <br/>
  $ kubectl apply -f cats.yaml <br/>
  $ kubectl apply -f dogs.yaml <br/>
  $ kubectl apply -f birds.yaml <br/>
* Create the ingress resource <br/>
  $ kubectl apply -f ingress.yaml <br/>
* List pods, services and ingress <br/>
  $ kubectl get pods <br/>
  $ kubectl get svc <br/>
  $ kubectl get ingress <br/>
* Get the ingress controller service name (type LoadBalancer) <br/>
  $ kubectl get svc -n ingress-basic <br/>
  ![image](https://user-images.githubusercontent.com/92582005/201920523-17d7702e-745b-43e3-bf4a-4db081c8f703.png) <br/>
* Get the URL ingress controller service by name we received in last step <br/>
  $ minikube service nginx-ingress-ingress-nginx-controller -n ingress-basic <br/>
  ![image](https://user-images.githubusercontent.com/92582005/201921310-66a51e64-4a4e-417a-aa81-b711043e6144.png) <br/>
* Browse to the cats, dogs and birds service by appending "/cats", "/dogs", "/birds". For example : <br/>
  $ http://192.168.59.100:30053/cats <br/>
  $ http://192.168.59.100:30053/dogs <br/>
  $ http://192.168.59.100:30053/birds <br/>
### Clean up <br/>
  $ kubectl delete ingressclass nginx <br/>
  $ kubectl delete all --all -n ingress-basic <br/>
  $ kubectl delete all --all -n ingress-nginx <br/>
  $ kubectl delete namespace ingress-basic <br/>
  $ kubectl delete namespace ingress-nginx <br/>
  $ kubectl delete all --all <br/><br/>
### References
* [What Is an Ingress Controller?](https://www.nginx.com/resources/glossary/kubernetes-ingress-controller/)</br>
 
  
