
# k8s-repo


## 1. dev-tools
```
kubectl run dev-tools -it --rm --image saturn203/austine-devtools

```
---
## 2. simple-api
```
// with namespace : api
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api-ns.yaml

kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/ns-api.yaml
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api.yaml

//ingress 
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/ingress-simple-api.yaml


endpoint : /api/hello, /api/simple , /api/version
```

---
## 3. nginx
### with yaml
```
// with namespace : nginx
kubectl apply -f  https://raw.githubusercontent.com/io203/k8s-repo/main/nginx/nginx-ns.yaml
kubectl apply -n nginx -f  https://raw.githubusercontent.com/io203/k8s-repo/main/nginx/nginx-loadbalancer-ns.yaml


kubectl create ns nginx
kubectl apply -n nginx -f  https://raw.githubusercontent.com/io203/k8s-repo/main/nginx/nginx.yaml

```

### kubectl 
```
kubectl create deployment nginx --image=nginx 
kubectl expose deploy nginx --port=80 --target-port=80 
kubectl expose deploy nginx --port 80 --target-port=80 --type LoadBalancer 
k port-forward svc/nginx 8085:80 

```
---
## 4. httpbin.org 사용
- http://httpbin.org/ip Origin IP를 반환합니다.
- http://httpbin.org/user-agent user-agent를 반환합니다.
- http://httpbin.org/headers 헤더 dict를 반환합니다.
- http://httpbin.org/get GET 데이터를 반환합니다.
- http://httpbin.org/post POST 데이터를 반환합니다.
- http://httpbin.org/put PUT 데이터를 반환합니다.
- http://httpbin.org/delete DELETE 데이터를 반환
- http://httpbin.org/gzip gzip으로 인코딩 된 데이터를 - 반환합니다.
- http://httpbin.org/status/:code 지정된 HTTP 상태 코드를 - 반환합니다.
- http://httpbin.org/response-headers?key=val 주어진 응답 - 헤더를 반환합니다.
- http://httpbin.org/redirect/:n 302 n 번 리디렉션합니다.
- http://httpbin.org/relative-redirect/:n 302 상대 - 리디렉션은 n 번 리디렉션됩니다.
- http://httpbin.org/cookies 쿠키 데이터를 반환합니다.
- http://httpbin.org/cookies/set/:name/:value 간단한 쿠키를 - 설정합니다.
- http://httpbin.org/basic-auth/:user/:passwd HTTPBasic - 인증에 도전합니다.
- http://httpbin.org/hidden-basic-auth/:user/:passwd 404’d - BasicAuth.
- http://httpbin.org/digest-auth/:qop/:user/:passwd HTTP - 다이제스트 인증에 도전합니다.
- http://httpbin.org/stream/:n n–100 회선을 스트리밍합니다.
- http://httpbin.org/delay/:n n–10 초 동안 응답이 지연됩니다.

---
## 5 install docker 
```
curl -fsSL https://get.docker.com/ | sudo sh ; sudo usermod -a -G docker $USER

//재기동없이 재접속하면 된다
```
---
## 6 install docker-compose 
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose ; sudo chmod +x /usr/local/bin/docker-compose ; docker-compose version

```