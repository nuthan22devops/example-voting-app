# Example Voting App through GCP kubernetes cluster

This project uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.
KB
architecture.excalidraw.png



 vote service will be run on port http://xx.xx.xx.xx:8080, and the `results` will be on http://xx.xx.xx.xx:8081 using ingress 


## Run the app services in google cloud Kubernetes cluster:

created Kubernetes cluster on GCP through standard configuration rather than autopilot
configured 2 nodes only as it was a free tier account
installed and logged into GCP cli through local host,
and navigated to projects list and set up the kubernetes cluster 
cloned the repository into local -> cd into the repository

The folder k8s-specifications contains the YAML specifications of the Voting App's services.
run the Kubectl apply -f k8s-specifications/ 
ALL THE K8S MANIFEST FILES HAS BEEN DEPLOYED
VERIFY THE ALL SERVICES THROUGH RUNNING Kubectl get all

<img width="865" height="447" alt="image" src="https://github.com/user-attachments/assets/e877155c-e92b-45f1-ab31-9469d6f93721" />

Expose the application voting service using INGRESS

kubectl create namespace ingress-nginx

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer

voting-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: vote-ingress
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: vote
            port:
              number: 8080

and access vote service through ingress loadbalancer ip address

  <img width="1258" height="553" alt="image" src="https://github.com/user-attachments/assets/7192ff21-94f9-40db-bf3e-064b20827a3f" />


Note: Results application cannot be exposed to public so we are accessing it through nodeport or kubectl portforward svc/result 8081:8081 on your browser.



