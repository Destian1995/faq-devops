
* the connection to the server localhost:8080 did you your specify host or posrt?
```
kubectl config view --raw > $HOME/.kube/config
```
* ошибки связанные с ClusterDNS 
```
Необходимо проверять настроен ли(включен ли) DNS, например в microk8s это аддон dns, отвечающий за CoreDNS
```
