# Services

### Comandos

Criar service por linha de comando:
```
kubectl run nginx --image nginx --port=80
kubectl expose pod nginx
```

Se verificarmos o nosso service, veremos que o tipo será ClusterIP, ou seja, nosso pod só estará disponível para dentro do cluster
```
kubectl get services
```
![ClusterIP](../images/notready.png)

Aumentando o número de replicas
```
kubectl scale --replicas=10 deployment nginx
```
