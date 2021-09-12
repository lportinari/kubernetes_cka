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
- Atualizar os pacotes da sua distro nos 3 nodes
- Desabilitar a swap
```
swapoff -a
```