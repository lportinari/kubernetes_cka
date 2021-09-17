# Question 1

- Criar um pod utilizando a imagem do Nginx versao 1.18.0, com o nome de giropops no namespace strigus.

## Resposta 
Nesse caso temos duas formas. A primeira utilizando somente linha de comando:
```
kubectl run giropops --image nginx:1.18.0 --port 80 --namespace strigus
```

## Resposta 2
A segunda, é a mais recomendada. Aqui podemos analisar, antes de já sair soltando o comando:
```
kubectl run giropops -- image nginx:1.18.0 --port 80 --namespace strigus --dry-run=client -o yaml > pod.yaml

kubectl create -f pod.yaml
```


# Question 2

- Aumentar a quantidade de replicas do deployment girus, que está utilizando a imagem no nginx 1.18.0, para 3 réplicas. O deployment está no namespace strigus.

## Resposta 1
```
kubectl scale deployment -n strigus girus --replicas 3
```

## Resposta 2
```
kubctl edit deployment -n strigus girus # lá dentro alteramos a quantidade de replicas e saimos
```

## Resposta 3
```
kubectl create deployment girus --image nginx:1.18.0 --port 80 --namespace strigus --replicas 3 --dry-run=client -o yaml > deployment2.yaml

kubectl apply -f deployment2.yaml
```


# Question 3

- Precisamos atualizar a versão do Nginx no pod gipops. Atualmente está na versão 1.18.0 e precisamos atualizar para versão 1.21.1

## Resposta 1
```
kubectl edit pod -n strigus giropops # Mudar a versão do Nginx dentro do yaml
kubectl describe -n strigus giropops 
```
## Resposta 2
```
kubectl set image pod giropops -n strigus web=nginx:1.21.1
```

# Dicas
alias k=kubectl