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


Homework 2. Kubernetes-controllers
==================================
1. Развернут кластер в **kind**
   ```
   kind create cluster --config kind-config.yaml
   ```
1. Создаем ReplicaSet для frontend приложения в кластере. Проверяем работу RS (scale, delete pod, apply нового манифеста с новым значением replica).
   ```
   kubectl apply -f frontend-replicaset.yaml | kubectl get pods -l app=frontend -w
   ```
1. Если в манифесте RS изменить tag образа ничего не произойдет, т.к. RS следит только за количеством PODs.
1. Создан Deployment для Paymentservice.
1. При обновлении манифеста Deployment изменения автоматически раскатываются в кластере
1. На примере созданного Deployment проверен механизм rollback
   ```
   kubectl rollout history deployment payment
   kubectl rollout undo deployment payment --to-revision=1 | kubectl get rs -l app=paymentservice -w
   ```