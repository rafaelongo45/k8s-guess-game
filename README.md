# **Kubernetes Guess Game**

Este repositório contém os manifestos para rodar a aplicação **Guess Game** usando Kubernetes. Ele é composto por:

- **Frontend**: React.
- **Backend**: Flask.
- **Banco de Dados**: PostgreSQL.
- **NGINX**: Proxy reverso.
- **Horizontal Pod Autoscaler (HPA)** para o backend.


## **Pré-requisitos**

Certifique-se de ter os seguintes itens instalados em sua máquina:

1. **kubectl** - CLI do Kubernetes.
2. **minikube** - Ferramenta para rodar Kubernetes localmente.
3. **Docker** - Para criar e gerenciar imagens de contêiner.
4. **git** - Para clonar este repositório.

### **Configuração do Kubernetes**

#### 1. Inicialização do Minikube:
```
minikube start --driver=docker
```
#### 2. Habilitar o metrics-server:

Esse passo é necessário para que o HPA funcione corretamente.

```
minikube addons enable metrics-server
```

### Configuração de domínio:

A aplicação foi configurada para rodar no domínio ```dominio.rfl.com```. Portanto, 
será necessário a alteração do arquivo hosts do SO.

```
minikube addons enable metrics-server
```
#### 1. Obtendo o IP do Minikube:

Basta rodar o comando abaixo:

```
minikube ip
```
#### 2. Atrelando o IP ao domínio:

Abra o arquivo /etc/hosts como administrador:

```
sudo nano /etc/hosts
```

Adicione a linha abaixo, substituindo <minikube_ip> pelo IP retornado no comando anterior.

```
<minikube_ip> dominio.rfl.com
```
Salve e feche o arquivo.

### Clonando o Repositório

```
git clone git@github.com:rafaelongo45/k8s-guess-game.git

cd k8s-guess-game
```
### Criação dos recursos

Após realizar as etapas anteriores, sua máquina está pronta para criar os recursos necessários para a aplicação. Para isso, certifique-se que está dentro do diretório do projeto clonado e rode os comandos abaixo.

```
kubectl apply -f ingress.yaml
kubectl apply -f db.yaml
kubectl apply -f api.yaml
kubectl apply -f frontend.yaml
kubectl apply -f nginx.yaml
```

### Acessando a aplicação.

Certifique-se de que todos os pods estão rodando.

```
kubectl get pods
```

Se todos estiverem com o status "Running", será possível acessar a aplicação. Para isso, abra um navegador e acesse:

```
http://dominio.rfl.com:30004
```

O HPA configurado pode ser verificado rodando o comando:

```
kubectl describe hpa api-hpa
```