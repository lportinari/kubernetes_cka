# Instalando o kubernetes

## Pré requisitos
* 2 GB ou mais por máquina.
* 2 CPUs ou mais.
* Conectividade total entre todas as máquinas do cluster (Rede pública ou privada).
* Hostname, MAC address e product_uuid exclusivos para cada nó.
* Desabilitar o Swap. É necessário desabilitar o swap para que o kubelet funcione corretamente.
* Portas que serão expostas:

![Kubernetes ports](../images/k8s_ports.PNG)

## **Passo 1:** Desabilitando a Swap
Executar nos 3 nodes:
- Atualizar os pacotes da sua distro nos 3 nodes
- Desabilitar a swap: `swapoff -a`
- Comentar a linha da swap no arquivo fstab, para que quando as máquinas forem rebootadas a swap não seja habilitada: `vim /etc/fstab`

## **Passo 2:** Instalando o Docker
Executar nos 3 nodes:
- Instalando o Docker: `curl -fsSL https://get.docker.com | bash`
- Conferindo se a instalação foi executada com sucesso: `docker --version`

## **Passo 3:** Adicionando o respositório do Kubernetes
Executar nos 3 nodes:
- Importando a chave: `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -`
- Adicionando o repositório: `echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list`
- Após adicionar o repositório, vamos rodar `apt-get update` para fazer o download da listagem de pacotes disponíveis desse novo repositório.
