# k8s_role_rolebinidng
Kubernetes Read-Only Role &amp; Rolebinding



#####Using RBAC Cluster Role and rolebinding on K8S cluster level.

##login any K8S master Server
## Create new user "developer" # for developer
useradd developer

openssl genrsa -out developer-sg.key 2048
openssl req -new -key developer-sg.key -out developer-sg.csr -subj "/CN=developer/O=root"

##ca.pem and ca.key files located in your k8s master
openssl x509 -req -in developer.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out developer-sg.crt -days 500
kubectl config set-credentials developer --client-certificate=developer-sg.crt  --client-key=developer-sg.key

##make sure about clustername, you can verify from admin config file.
kubectl config set-context developer --cluster=singapore --user=developer

##Create new yaml file with name "cluster-role.yaml"
kubectl create -f cluster-role.yaml


##create a new config file
vim config

###################################################################################################
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: ******paste your certificate here ****
    server: paste your k8s cluster ip or url
  name: singapore
contexts:
- context:
    cluster: singapore
    user: developer
  name: developer
current-context: developer
kind: Config
preferences: {}
users:
- name: developer
  user:
    client-certificate: /root/.kube/developer.crt
    client-key: /root/.kube/developer.key

###################################################################################################

Once everything done, just copy the below files to your home directory /home/user/.kube/.
config, developer.crt, developer.key

run:
#kubectl get pods --all-namespaces

###################################################################################################


