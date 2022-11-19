# kubernetes-pratices

## Basic commands
* Switch context
```
kubectl config get-contexts
kubectl config use-context <context-name>
```
* Switch namespace
```
kubectl get ns
kubectl config set-context --current --namespace=<namespace>
```
* Get all info (pod, deployment, service, ...)
```
kubectl get all -o wide
```

## Namespace
* Get namespace (short name = 'ns')
```
kubectl get namespaces
[kubectl get ns]
```
* Describe namespace
```
kubectl describe ns <namespace>
```
* Create new namespace
```
apiVersion: v1
kind: Namespace
metadata:
  name: <insert-namespace-name-here>

# run commnad: kubectl create -f ./<my-file>.yaml
```
alternatively, you can create namespace using below command
```
kubectl create namespace <namespace>
```
* Delete namespace
```
kubectl delete namespace <namespace>
```

## Pod
* Get pods
```
kubectl get po [-o wide]
```
* Create pod
```
kubectl apply -f <pod.yaml>
```
* Edit pod
```
kubectl edit po/<pod-name>
```
* Delete pod
```
kubectl delete po <pod-name>
kubectl delete -f <pod.yaml>
```
* Pod detail
```
kubeclt describe po/<pod-name>
kubectl get po/<pod-name> -o yaml
```
* Pod's log
```
kubectl logs po/<pod-name> [-f]
```
* Execute command in pod
```
kubectl exec [-it] <pod-name> <command>
```

## Deployent
* Get deployments
```
kubectl get deploy [-o wide]
```
* Create new deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  # tên của deployment
  name: deployapp
spec:
  # số POD tạo ra
  replicas: 3

  # thiết lập các POD do deploy quản lý, là POD có nhãn  "app=deployapp"
  selector:
    matchLabels:
      app: deployapp

  # Định nghĩa mẫu POD, khi cần Deploy sử dụng mẫu này để tạo Pod
  template:
    metadata:
      name: podapp
      labels:
        app: deployapp
    spec:
      containers:
      - name: node
        image: ichte/swarmtest:node
        resources:
          limits:
            memory: "128Mi"
            cpu: "100m"
        ports:
          - containerPort: 8085
```
Run command: ```kubectl apply -f <file-name>.yaml```
* Edit deployment
```
kubectl edit deploy/<deploy-name>
```
* Deployment description
```
kubectl describe deploy/<depoy-name> [-o wide/yaml..]
```
* Update image
```
kubectl set image deploy/<deploy-name> <old-image>=<new-image> --record
```
* Scale pods
```
kubectl scale deploy/<deploy-name> --replicas=<number>
```