# Kubernetes Read-Only Role &amp; Rolebinding



##### Using RBAC Cluster Role and rolebinding on K8S cluster level.

##login any K8S master Server
##Create new user "developer" # for developer
useradd developer

openssl genrsa -out developer-sg.key 2048
openssl req -new -key developer-sg.key -out developer-sg.csr -subj "/CN=developer/O=root"

##ca.pem and ca.key files located in your k8s master
openssl x509 -req -in developer.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out developer-sg.crt -days 500
kubectl config set-credentials developer --client-certificate=developer-sg.crt  --client-key=developer-sg.key

##make sure about clustername, you can verify from admin config file.
kubectl config set-context developer --cluster=singapore --user=developer

##Make sure you downloaded my cluster-role.yaml file. 
kubectl create -f cluster-role.yaml


##create a new config file as per the attached sample


Once everything done, just copy the below files to your home directory /home/user/.kube/.
config, developer.crt, developer.key

run:
#kubectl get pods --all-namespaces




