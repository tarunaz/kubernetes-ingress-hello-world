sudo minikube start

minikube addons enable ingress
kubectl delete deployment,service echoserver
kubectl delete ingress kubernetes-ingress-hello-world

eval $(sudo minikube docker-env)
docker build -t hello-service hello-service/ 
docker run -ti hello-service

kubectl run echoserver --image=gcr.io/google_containers/echoserver:1.4 --port=8080
kubectl expose deployment echoserver --type=NodePort
sudo minikube service echoserver

kubectl run munich --image=hello-service:latest --image-pull-policy=Never --env="GREETINGS=grias-eich" --port=80
kubectl expose deployment munich --type=NodePort
sudo minikube service munich

kubectl run paris --image=hello-service:latest --image-pull-policy=Never --env="GREETINGS=bonjour" --port=80
kubectl expose deployment paris --type=NodePort
sudo minikube service paris

minikube addons enable ingress
kubectl create -f kubernetes-ingress-hello-world.yaml
kubectl describe ing kubernetes-ingress-hello-world

echo "$(sudo minikube ip) echo.toall greetings.toall" | sudo tee -a /etc/hosts

# recommended readings:
https://medium.com/@Oskarr3/setting-up-ingress-on-minikube-6ae825e98f82
https://github.com/kubernetes/ingress-nginx/tree/master/deploy#minikube
https://github.com/nginxinc/kubernetes-ingress/blob/master/docs/nginx-ingress-controllers.md
https://medium.com/@gokulc/setting-up-nginx-ingress-on-kubernetes-2b733d8d2f45


