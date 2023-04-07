FAQ kuber, Linux.
---

* the connection to the server localhost:8080 did you your specify host or posrt?
Чтобы исправить это введите копирования конфига в локал дерикторию.
```
kubectl config view --raw > $HOME/.kube/config
```
* ошибки связанные с ClusterDNS 
```
Необходимо проверять настроен ли(включен ли) DNS, например в microk8s это аддон dns, отвечающий за CoreDNS
```
* Падает контейнер с multitool, необходимо добавить в контейнер следующую команду, чтобы он не падал после запуска.
```
command: [ "/bin/bash", "-ce", "tail -f /dev/null" ]
```
Итоговый вариант такой:
```
  - name: multitool
        image: wbitt/network-multitool
        ports:
        - containerPort: 8080
        command: [ "/bin/bash", "-ce", "tail -f /dev/null" ]
```
