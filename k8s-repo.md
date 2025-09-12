
# k8s-repo


## 1. dev-tools
```
kubectl run curl -it --rm --image curlimages/curl -- sh


kubectl run dev-tools -it --rm --image saturn203/austine-devtools
https://github.com/io203/k8s-repo/blob/main/k8s-repo.md
```
---
## 2.1 simple-api
```
// simple-api + api-namespace 
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api-ns.yaml

// api namespace 생성만 한다
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/ns-api.yaml
// simple-api 만 생성 ( namespace는 api로 할당되어 있다 )
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/simple-api.yaml

//ingress (simple-api.3.39.19.175.sslip.io)
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-api/ingress-simple-api.yaml


endpoint : /api/hello, /api/simple , /api/version
```
simple-api-ingress
```
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-api-ingress
  namespace: api
spec:
  ingressClassName: "nginx"
  rules:
  - host: "simple-api.3.39.19.175.sslip.io"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service: 
            name: simple-api-svc
            port: 
              number: 8080
EOF

```

## 2.2 simple-fe
```
// with namespace : fe
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-fe/simple-fe-ns.yaml

kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-fe/ns-fe.yaml
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-fe/simple-fe.yaml

//ingress (simple-api.3.39.19.175.sslip.io)
kubectl apply -f https://raw.githubusercontent.com/io203/k8s-repo/main/simple-fe/ingress-simple-fe.yaml

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
### ingress-nginx
```
cat <<EOF | kubectl apply -f - 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  namespace: nginx  
spec:
  ingressClassName: "nginx"
  rules:
  - host: "nginx.192.168.41.34.sslip.io"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service: 
            name: nginx-svc
            port: 
              number: 80
EOF
```

## 4. sleep pod (curl)
```
//설치 
kubectl apply -n api -f  https://raw.githubusercontent.com/io203/k8s-repo/main/tools/sleep.yaml

//확인
k -n api exec -it deploy/sleep -- curl -s https://jsonplaceholder.typicode.com/posts

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

## Free fake API for testing and prototyping
- https://jsonplaceholder.typicode.com
```
curl https://jsonplaceholder.typicode.com/posts
```

## mise.toml
```toml
[tools]
python = "3.12.9"

[tasks.dev]
description = "Enter development environment"
run = '''
if [ ! -d ".venv" ]; then
    echo "Creating virtual environment..."
    python -m venv .venv
fi

echo "Activating virtual environment..."
source .venv/bin/activate

# 프롬프트 확실히 설정
export VIRTUAL_ENV_DISABLE_PROMPT=0

# 가상환경 활성화 확인
echo "Virtual environment activated: $(basename $VIRTUAL_ENV)"
echo "Python path: $(which python)"

# 새 쉘 시작
exec $SHELL
'''
```
