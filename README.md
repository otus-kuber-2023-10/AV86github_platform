# AV86github_platform
AV86github Platform repository


Homework 1. Kubernetes-intro
============================

1. Развернут кластер в **minikube**
   ```
   minikube start
   ```
 1. Почему после удаления все поды восстанавливаются
    - *coredns* - управляется Deployment
 	 - *kubeproxy* - управляется DaemonSet
 	 - остальные компоненты критичные для системы и напрямую управляется *kubelet*
    
      ```
      minikube ssh
      cd /etc/kubernetes/manifests
      ```
 1. Собираем, запускаем nginx
    
 		```
 		docker build -t avolchkov/otus-nginx:1.0 .
 		docker run -d -p 8000:8000 --name otus-nginx avolchkov/otus-nginx:1.0 .
 		```
 1. Создаем манифесты для поды на основе собранного образа и запускаем в кубе
 1. Собираем front hipstershop и запускаем его в minikube
    
 		```
 		kubectl run frontend --image avolchkov/hipster-front:latest --restart=Never
 		```
 1. Почему не запустилась пода?
    - В логах видим что не хватает переменной *PRODUCT_CATALOG_SERVICE_ADDR*
 	 - Генериуем manifest сломанной поды
 	 - Добавляем необходимую переменную
 	 - Создаем манифест и применяем его
    
 		```
 		kubectl logs frontend
 		kubectl run frontend --image avolchkov/hipster-front:latest --restart=Never --dry-run -o yaml >front.yaml
 		kubectl delete pod  frontend
 		kubectl apply -f frontend-pod-healthy.yaml

 		```
