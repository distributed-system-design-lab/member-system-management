## Part 1：NGINX Ingress Controller 配置
```bash
wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.2/deploy/static/provider/cloud/deploy.yaml
# 修改里面Service name: ingress-nginx-controller 的一段，找到type：type 改为NodePort ，因为你没有LoadBalance可测试。
kubectl apply -f deploy.yaml
kubectl get pods --namespace=ingress-nginx
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
kubectl get pods --namespace=ingress-nginx -o wide

kubectl create deployment gate --image=httpd --port=80
kubectl expose deployment gate
kubectl create ingress gate --class=nginx \
  --rule peoplesystem.tatdvsonorth.com/=gate:80

sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
# 在spec.containers.command的最后面加上 --service-cluster-ip-range 这一行，如下内容- --service-node-port-range=1-65535
sudo systemctl daemon-reload
sudo systemctl restart kubelet
kubectl apply -f deploy.yaml

```
<img width="949" alt="image" src="https://github.com/user-attachments/assets/d34833ac-08fe-4e1c-b16d-18802f945e21">

## Part 2：Istio 配置
<img width="1213" alt="image" src="https://github.com/user-attachments/assets/5b105398-2a93-4a40-81ba-7037e72feb73">

## 參考
- 東西向 Istio: `https://www.soulchild.cn/post/2660/`
- 南北向 Nginx Ingress Controller: `https://blog.csdn.net/shelutai/article/details/123904190`
