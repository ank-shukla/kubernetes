kubectl run nginx --image=nginx
kubectl describe nginx

kubectl explain pods --recursive # get details on all fields, kind of blueprint

kubectl run redis --image=redis -n finance
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
kubectl run nginx-pod --image=nginx:alpine
kubectl create -f redis-definition.yaml

kubectl get pods -o wide/name/json/yaml --no-headers
kubectl get all --selector env=prod,bu=finance,tier=frontend
kubectl get all -l env=prod,bu=finance,tier=frontend

kubectl get pods --selector env=dev

kubectl create deployment blue --image=nginx --replicas=3

kubectl edit pod redis
kubectl apply -f redis-definition.yaml

kubectl replace -f elephant.yaml --force

kubectl explain replicaset | grep VERSION
kubectl scale replicaset --replicas=5 new-replica-set

kubectl delete replicaset <replicaset-name>
kubectl delete -f <file-name>.yaml

####Entry Point and CMD in Docker corresponds to Command and Args in Kubernetes

kubectl create configmap webapp-config-map --from-literal=APP_COLOR=darkblue

kubectl exec ubuntu-sleeper -- whoami
kubectl -n elastic-stack exec -it app -- cat /log/app.log
kubectl exec webapp -- cat /log/app.log

kubectl describe node controlplane | grep -i taints
kubectl taint nodes node01 spray=mortein:NoSchedule

kubectl label node node01 color=blue

kubectl logs <podname> -c <int container name> #get logs from init containers
lubectl logs -f <podname> <container name>

kubectl create deployment nginx --image=nginx:1.16
kubectl edit deployment frontend
kubectl rollout status deployment nginx
kubectl rollout history deployment nginx
kubectl rollout history deployment nginx --revision=1

##We can use the --record flag to save the command used to create/update a deployment against the revision number.
kubectl create -f deployment-def.yaml --record
kubectl set image deployment nginx nginx=nginx:1.17 --record

kubectl rollout undo deployment nginx

kubectl create job throw-dice-job --image=kodekloud/throw-dice --dry-run=client -o yaml > throw-dice-job.yaml
kubectl get job
watch "kubectl get job"
kubectl describe jobs.batch throw-dice-job

kubectl describe svc
kubectl expose deployment simple-webapp-deployment --name=webapp-service --target-port=8080 --type=NodePort --port=8080 --dry-run=client -o yaml > svc.yam

kubectl get ingress --all-namespaces
kubectl describe ingress --namespace app-space
kubectl edit ingress --namespace app-space

kubectl create namespace ingress-space
kubectl create configmap nginx-configuration --namespace ingress-space
kubectl create serviceaccount ingress-serviceaccount --namespace ingress-space
kubectl -n ingress-space get roles
kubectl -n ingress-space get rolebindings
# create ingress controller deployment
# expose that deployment for ingress as service
kubectl expose -n ingress-space deployment ingress-controller --type=NodePort --port=80 --name=ingress --dry-run=client -o yaml > ingress.yaml


kubectl get networkpolicy
kubectl get netpol

kubectl get pv
kubectl get pvc
kubectl get pv,pvc

#storage class
kubectl get sc

docker images
docker build -t webapp-color .
docker run -p 8282:8080 webapp-color
docker run python:3.6 cat /etc/*release*

kubectl config view --kubeconfig my-kube-config
kubectl config --kubeconfig=/root/my-kube-config use-context research
kubectl config --kubeconfig=/root/my-kube-config current-context

kubectl proxy
:8001
:8001/apis

kubectl describe pod kube-apiserver-controlplane -n kube-system
kubectl get roles
kubectl get roles --all-namespaces
kubectl get roles -A
kubectl describe role kube-proxy -n kube-system
kubectl describe rolebinding kube-proxy -n kube-system
kubectl get pods --as dev-user
kubectl create role developer --namespace=default --verb=list,create,delete --resource=pods
kubectl create rolebinding dev-user-binding --namespace=default --role=developer --user=dev-user
kubectl edit role developer -n blue

kubectl api-resources

kubectl get clusterroles --no-headers | wc -l
kubectl get clusterroles --no-headers -o json | jq '.items | length'
kubectl get clusterrolebindings --no-headers | wc -l
kubectl get clusterrolebindings --no-headers -o json | jq '.items | length'\
kubectl describe clusterrolebinding cluster-admin
kubectl describe clusterrole cluster-admin

kubectl exec -it kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep 'enable-admission-plugins'
grep enable-admission-plugins /etc/kubernetes/manifests/kube-apiserver.yaml