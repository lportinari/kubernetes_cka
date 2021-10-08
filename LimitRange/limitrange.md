# Limit Range

Por padrão, os contêineres são executados com recursos de computação ilimitados em um cluster do Kubernetes. Com cotas de recursos, os administradores de cluster podem restringir o consumo de recursos e a criação com base no namespace. Em um namespace, um pod ou contêiner pode consumir a quantidade de CPU e memória definida pela cota de recursos do namespace. Existe a preocupação de que um pod ou contêiner possa monopolizar todos os recursos disponíveis. LimitRange é uma política para restringir as alocações de recursos (para pods ou contêineres) em um namespace.

Um LimitRange fornece restrições que podem:

* Aplique o uso mínimo e máximo de recursos de computação por pod ou contêiner em um namespace.
* Aplique a solicitação de armazenamento mínimo e máximo por PersistentVolumeClaim em um namespace.
* Imponha uma proporção entre a solicitação e o limite para um recurso em um namespace.
* Defina a solicitação / limite padrão para recursos de computação em um namespace e injete-os automaticamente em Containers em tempo de execução.


## Habilitando LimitRange
O suporte a LimitRange foi ativado por padrão desde o Kubernetes 1.10.
Um LimitRange é imposto em um namespace específico quando há um objeto LimitRange nesse namespace.
O nome de um objeto LimitRange deve ser um nome de subdomínio DNS válido.

## Visão geral do LimitRange
* O administrador cria um LimitRange em um namespace.
* Os usuários criam recursos como pods, contêineres e PersistentVolumeClaims no namespace.
* O controlador de admissão LimitRanger impõe padrões e limites para todos os pods e contêineres que não definem os requisitos de recursos de computação e rastreia o uso para garantir que não exceda o mínimo de recursos, o máximo e a proporção definida em qualquer LimitRange presente no namespace.
* Se criar ou atualizar um recurso (Pod, Container, PersistentVolumeClaim) que viole uma restrição LimitRange, a solicitação para o servidor da API falhará com um código de status HTTP 403 FORBIDDEN e uma mensagem explicando a restrição que foi violada.
* Se um LimitRange for ativado em um namespace para recursos de computação como CPU e memória, os usuários devem especificar solicitações ou limites para esses valores. Caso contrário, o sistema pode rejeitar a criação do pod.
* As validações LimitRange ocorrem apenas no estágio de admissão do pod, não em pods em execução.


Exemplos de políticas que podem ser criadas usando o Limit Range:
* Em um cluster de 2 nós com capacidade de 8 GiB de RAM e 16 núcleos, restrinja os pods em um namespace para solicitar 100 mi de CPU com um limite máximo de 500 mi para CPU e solicitar 200 Mi de memória com um limite máximo de 600 mi de memória.
* Defina o limite de CPU padrão e a solicitação de 150 m e a solicitação de memória padrão de 300 Mi para contêineres iniciados sem cpu e solicitações de memória em suas especificações.

No caso em que os limites totais do namespace são menores que a soma dos limites dos pods / contêineres, pode haver contenção de recursos. Nesse caso, os contêineres ou pods não serão criados.
Nem contenção nem alterações em um LimitRange afetarão os recursos já criados.


## Aplicando o LimitRange em um namespace
Criaremos um arquivo yaml definindo os nossos limites de recursos

```yaml
---
apiVersion: v1
kind: LimitRange
metadata:
  name: limitando-recursos
spec:
  limits:
  - default:
      cpu: 1
      memory: 256Mi
    defaultRequest:
      cpu: 0.5
      memory: 128Mi
    type: Container
```

Agora vamos aplicar no nosso namespace:
```
kubectl create -f namespace_limitado.yaml -n meu_namespace
```

Agora podemos rodar o comando abaixo para certificar se o Limitrane foi criado
```
kubectl get limitranges -n meu_namespace
```


