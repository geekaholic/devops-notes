# K8s commands
## Pod related
```bash
# To run a pod
kubectl run nginx —-image=nginx

# To monitor pods spinning up in default ns
kubectl get pods -n default w

# To get pods from all namespaces
kubectl get pods --all-namespaces

# To describe pod resource
kubectl describe pod —namespace cert-manager cert-manager-webhook-7b887475fb-zdvs5

# To execute a command in a pod (with one container)
kubectl exec -it nginx-demo-7d56b74b84-ptln2 — /bin/bash

# To execute a command in a pod with many containers use -c to specify container
kubectl exec -it nginx-demo-7d56b74b84-ptln2 -c nginx — /bin/bash

# To get pods with matching label
kubectl get pods -l run=kubernetes-bootcamp

# To show labels
kubectl get pods —show-labels

# Expose a pod as a service
kubectl expose pod redis --port=6379 --name redis-service

# Run pod and expose on a port
kubectl run custom-nginx --image=nginx --port=8080

# To forward a port from pod to localhost
kubectl port-forward pod/custom-nginx 8080

```

## Service related
```bash
# To forward port
kubectl port-forward -n default svc/kubeapps 8080:80

# To describe a service
kubectl describe service kubernetes-bootcamp

# To get services with matching label
kubectl get services -l run=kubernetes-bootcamp

```

## Deployment related
```bash
# To create a deployment
kubectl create deployment nginx-demo —image=nginx

# To get deployments
kubectl get deployments

# To expose deployment and create a service (using NodePort)
kubectl expose deployments/kubernetes-bootcamp —type=NodePort --target-port=8080 —-port=8080

# To edit a live deployment
kubectl edit deployment nginx

# To scale a deployment
kubectl scale deployment nginx -—replicas=5

# To update the image of an existing deployment
kubectl set image deployment nginx nginx=nginx:1.18

# To port forward a deployment to access from localhost
kubectl port-forward deployment/nginx 8080
```

## ReplicaSet related
```bash
# To get replicasets
kubectl get rs

# To edit a replicates
kubectl edit replicaset

# To scale a replicates
kubectl scale —replicas=2 rs/new-replica-set
```

## Namespace related
```bash

# To create a new context
kubectl create ns dev

# To switch default ns context
kubectl config set-context $(kubectl config current-context) --namespace dev
```

## Resource management
```bash

# To create using config
kubectl create -f nginx.yaml

# To delete using config
kubectl delete -f nginx.yaml

# To replace/update config
kubectl replace -f nginx.yaml

# To apply config
kubectl apply -f nginx.yaml

# To apply an entire folder
kubectl apply -f ./deployment

# To create a yaml using dry-run
kubectl run nginx —-image=nginx —-dry-run=client -o yaml > pod.yaml

# To skip showing header
kubectl get pods -—no-headers
```

## Taints and tolerations examples
```bash
# To taint a node to not schedule app=myapp key/val
kubectl taint nodes node01 app=myapp:NoSchedule

# To untaint a node
kubectl taint nodes node01 app=myapp:NoSchedule-

# To see taints on an existing node
kubectl describe node | grep -i taint
```

## Scheduling related
```bash
# To add a label to a node in order to schedule pods
kubectl label node node01 size=Large
```

## Cluster related
```bash
# View current kubeconfig
kubectl config view

# To get cluster info
kubectl cluster-info

# To check cluster health use componentstatuses (cs)
kubectl get componentstatuses

# To see all events from cluster
kubectl get events --all-namespaces

# Get contexts (i.e clusters configured)
kubectl config get-contexts

# To get current namespace context
kubectl config current-context

# Set current context
kubectl config use-context foo-cluster

# To get servicer accounts
kubectl get serviceaccount
```

## Storage related
```bash
# To get support storage classes
kubectl get storageclass

# To make storage class default
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

```


#k8s