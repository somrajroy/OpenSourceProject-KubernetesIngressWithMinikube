# Deploying applications using Ingress - Minikube <br/>
Deploying applications using Ingress using minikube. Good for quick testing, POC's, Demo etc purposes <br/>
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
* Deploy the NGINX ingress controller in minikube <br/>
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
  $ kubectl delete all --all <br/>
 
  
