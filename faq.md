FAQ kuber, Linux.
--

Здесь я описываю те ошибки с которыми сталкиваюсь при работе с kuber, linux, ansible и др.
И конечное решение в рамках которого ошибка устраняется.



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

* Не запускается контейнер после запуска сервиса.
Тут если нигде с label не накосячили, Service должен видеть свои pods через selector.
Если все ок то необходимо добавить в Deployment такую команду
```
command: ['sh', '-c', 'until nslookup nginx-service.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for nginx-service; sleep 2; done;']

```
Итоговый вариант
```
 spec:
      containers:
      - name: nginx
        image: nginx
      initContainers:
      - name: wait-for-service
        image: busybox
        command: ['sh', '-c', 'until nslookup nginx-service.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for nginx-service; sleep 2; done;']

```
