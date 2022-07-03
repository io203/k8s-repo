
# k8s-repo


## 1. dev-tools
```
kubectl run dev-tools -it --rm --image saturn203/austine-devtools

```
## 2. simple-api
```
// with namespace : api
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api-ns.yaml

kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/ns-api.yaml
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api.yaml
```

## 3. nginx
### with yaml
```
kubectl apply -f  https://raw.githubusercontent.com/io203/k8s-repo/main/nginx/nginx.yaml

```

### kubectl 
```
kubectl create deployment nginx --image=nginx 
kubectl expose deploy nginx --port=80 --target-port=80 
kubectl expose deploy nginx --port 80 --target-port=80 --type LoadBalancer 
k port-forward svc/nginx 8085:80 

```