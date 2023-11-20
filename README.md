# AV86github_platform
AV86github Platform repository


Homework 1. Kubernetes-intro
============================

1. Развернут кластер в **minikube**
   ```
   minikube start
   ```

 1. Почему после удаления все поды восстанавливаются
 		2. *coredns* - управляется Deployment
 		2. *kubeproxy* - управляется DaemonSet
 		3. остальные компоненты критичные для системы и напрямую управляется *kubelet*
 			```
 			minikube ssh
 			cd /etc/kubernetes/manifests

 			```
