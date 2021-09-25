### Question 1
Precisamos levantar algumas informações sobre o nosso cluster.
- Quantos nodes são workers?
2
- Quantos nodes são master?
1
- Qual o Pod Network (CNI)?
kube-flannel 
- Qual oCIDR dos pods no segundo worker?
```
kubectl describe nodes nodename | grep PodCIDR
```
10.244.0.128/25
- Qual o DNS que estamos utilizando no cluster?
coredns

Adicionar as informações coletadas no /opt/cluster_info.txt


### Questão 2
Precisamos criar um pod com as seguintes caracteristicas:

- Precisa ter um container rodando a imagem do Nginx, com um volume montado no
  diretorio html do nginx.

- Precisa ter um outro container rodando busybox e adicionando algum conteúdo
  ao arquivo /tmp/index.html

```bash
      => command: ["sh", "-c", "while true; do uname -a >> /tmp/index.html; date >> /tmp/index.html; sleep 2; done"]
```

- Precisamos ter um outro container rodando o busybox e executando o seguinte
  comando:
      => command: ["sh", "-c", "tail -f /tmp/index.html"]

### Resposta 2

Criamos o arquivo pod.yaml e nele adicionamos os tres containers

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: meu-pod
spec:
  containers:
    - name: container-1
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: workdir
          mountPath: /usr/share/nginx/html
      resources:
        limits:
          memory: '1Gi'
          cpu: '800m'
        requests:
          memory: '700Mi'
          cpu: '400m'
    - name: container-2
      image: busybox
      command: ["sh", "-c", "while true; do uname -a >> /tmp/index.html; date >> /tmp/index.html; sleep 2; done"]
      volumeMounts:
        - name: workdir
          mountPath: /tmp/
    - name: container-3
      image: busybox
      command: ["sh", "-c", "tail -f /tmp/index.html"]
      volumeMounts:
        - name: workdir
          mountPath: /tmp/
  # These containers are run during pod initialization
  dnsPolicy: Default
  volumes:
    - name: workdir
      emptyDir: {}
```

Criando o pod
```bash
k create -f pod.yaml
```

Verificando os logs par ver tudo funcionando

```bash
k logs -f meu-pod container-1
k logs -f meu-pod container-2
k logs -f meu-pod container-3
k exec -ti meu-pod -c container-1 -- bash
```