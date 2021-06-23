# ssl_kubernetes
setup ssl in kubernetes ingress

Securing the Ingress Using Cert-Manager
1) kubectl create namespace cert-manager
2) helm repo add jetstack https://charts.jetstack.io
3) helm repo update
4) helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.2.0 --set installCRDs=true
 
        Output
		NAME: cert-manager
		LAST DEPLOYED: Sun Dec 13 11:29:32 2020
		NAMESPACE: cert-manager
		STATUS: deployed
		REVISION: 1
		TEST SUITE: None
		NOTES:
		cert-manager has been deployed successfully!
 
5) nano production_issuer.yaml

      Attached the below description 
      
6) kubectl apply -f production_issuer.yaml

           Output 
            clusterissuer.cert-manager.io/letsencrypt-prod created
7) nano kubernetes-ingress.yaml


      Attached the yaml file below description 
      
      Output
      
          ingress.networking.k8s.io/kubernetes-ingress configured
	  
8) kubectl apply -f kubernetes-ingress.yaml
9) kubectl describe certificate hello-kubernetes-tls


production_issuer.yaml

apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    # Email address used for ACME registration
    email: your_email_address
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # Name of a secret used to store the ACME account private key
      name: letsencrypt-prod-private-key
    # Add a single challenge solver, HTTP01 using nginx
    solvers:
    - http01:
        ingress:
          class: nginx
	  

