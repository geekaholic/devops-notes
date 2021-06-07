# K8s sample yaml examples

## Pod examples
### General case
```yaml
apiVersion: v1
kind: Pod
metadata:
        name: myapp-pod
        namespace: dev
        labels:
                app: myapp
                labels:
                        app: myapp
                        type: front-end
        spec:
                containers:
                - name: nginx-continer
                  image: nginx
```

### Explicitly scheduling to run on a node
```yaml

apiVersion: v1
kind: Pod
metadata:
        name: nginx
        labels:
                name: nginx

spec:
        containers:
        - name: nginx
    image: nginx
    ports:
      - containerPort: 8080
  nodeName: node02
```

### Schedule Pod on Node01
Node01 needs to have a label set on it

```yaml
apiVersion: v1
kind: Pod
metadata:
        name: myapp-pod

spec:
        containers:
  - name: data-processor
    image: data-processor

  nodeSelector:
    size: Large
```

### Schedule Pod using Node Affinity
Node Affinity gives more flexibility compared to label based selectors but is also more complex.

```yaml
apiVersion: v1
kind: Pod
metadata:
        name: myapp-pod

spec:
        containers:
  - name: data-processor
    image: data-processor

  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      #preferredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            #operator: NotIn
            #operator: Exists
            values:
            - Large
            - Medium

```

## ReplicaSet example
```yaml
apiVersion: app/v1
kind: ReplicaSet
metadata:
        name: myapp-rs
        labels:
                app: myapp
                type: front-end
spec:
        template:
                metadata:
                        name: myapp-pod
                        labels:
                                app: myapp
                                type: front-end
                spec:
                        containers:
                                - name: nginx-container
                                  image: nginx
        replicas: 3
        selector:
                matchLabels:
                        type: front-end  
```

## Deployment example
```yaml
apiVersion: app/v1
kind: Deployment
metadata:
        name: myapp-deployment
        labels:
                app: myapp
                type: front-end
spec:
        template:
                metadata:
                        name: myapp-pod
                        labels:
                                app: myapp
                                type: front-end
                spec:
                        containers:
                                - name: nginx-container
                                  image: nginx
        replicas: 3
        selector:
                matchLabels:
                        type: front-end 
```

## Service examples
### NodePort type example

```yaml
apiVersion: v1
kind: Service
metadata:
        name: myapp-service

spec:
        type: NodePort
        ports:
                - targetPort: 80
                  port: 80
                  # nodePort should be > 30000 - < 32,768
                  nodePort: 30008
        selector:
                app: myapp
                type: front-end
```

### ClusterIP type example

```yaml
apiVersion: v1
kind: Service
metadata:
        name: back-end

spec:
        type: ClusterIP
        ports:
                - targetPort: 80
                  port: 80
        selector:
                app: myapp
                type: back-end
```

### LoadBalancer type example
Only works on support cloud platforms (not on premise)

```yaml
apiVersion: v1
kind: Service
metadata:
        name: myapp-service

spec:
        type: LoadBalancer
        ports:
                - targetPort: 80
                  port: 80
                  nodePort: 30008
``` 

## Namespace example
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

## ResourceQuota example
```yaml
apiVersion: apps/v1
kind: ResourceQuota
metadata:
        name: compute-quota
        namespace: dev
spec:
        hard:
                pods: "10"
                requests.cpu: "4"
                requests.memory: 5Gi
                limits.cpu: "10"
                limits.memory: 10Gi
```

## Annotation example
Add annotations to `metadata` section.
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels:
    app: App1
  annotations:
    buildVersion: 1.34
...
```

## Toleration example
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "myapp"
    effect: "NoSchedule"
```

## Reference links
[kubectl Usage Conventions | Kubernetes](https://kubernetes.io/docs/reference/kubectl/conventions/)


#k8s
