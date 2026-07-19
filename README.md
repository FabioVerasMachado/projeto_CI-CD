markdown

# 🎓 Mundo Invertido - CI/CD com GitLab + Kubernetes

![DevOps](https://img.shields.io/badge/DevOps-Pipeline-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-blue)
![GitLab](https://img.shields.io/badge/GitLab-CI%2FCD-orange)
![Docker](https://img.shields.io/badge/Docker-26.0-blue)

## 📋 Sobre o Projeto

Este projeto é uma demonstração prática de um pipeline **CI/CD completo** utilizando:

- **GitLab** como repositório e orquestrador de CI/CD
- **Kubernetes (Minikube)** para orquestração de containers
- **Docker** para containerização da aplicação
- **Docker Hub** como registro de imagens

O projeto implementa o site "Mundo Invertido" (baseado na série Stranger Things) com um pipeline automatizado que:

1. 🔨 **Builda** a imagem Docker automaticamente
2. 📤 **Envia** para o Docker Hub
3. 🚀 **Deploya** no Kubernetes com zero downtime

---

## 🏗️ Arquitetura do Projeto

[Desenvolvedor] → [GitLab] → [Pipeline CI/CD] → [Kubernetes] → [Site no Ar]
↓ ↓ ↓ ↓ ↓
Commit Repositório Build + Deploy Orquestração Acessível 24/7
text


### 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Finalidade |
|------------|--------|------------|
| **GitLab** | 19.2.0 | CI/CD + Repositório |
| **Kubernetes** | 1.28 | Orquestração de containers |
| **Minikube** | v1.35.0 | Cluster Kubernetes local |
| **Docker** | 26.0 | Containerização |
| **Docker Hub** | - | Registro de imagens |
| **Apache HTTP Server** | latest | Servidor Web |

---

## 📁 Estrutura do Projeto

projetominikube/
├── .gitlab-ci.yml # Pipeline CI/CD
├── Dockerfile # Configuração da imagem Docker
├── deployment.yaml # Deployment no Kubernetes
├── service.yaml # Service LoadBalancer
├── hpa.yaml # Horizontal Pod Autoscaler
├── values.yaml # Configuração do GitLab Runner
├── kubeconfig.yaml # Conexão com o cluster
├── index.html # Página principal
├── assets/ # CSS, JS, imagens e músicas
│ ├── css/
│ ├── js/
│ ├── images/
│ └── musics/
└── README.md # Documentação
text


---

## 🚀 Pipeline CI/CD

### O que o pipeline faz:

| Stage | Job | Descrição |
|-------|-----|-----------|
| **build** | build | Constrói a imagem Docker e envia para o Docker Hub |
| **deploy** | deploy | Atualiza o deployment no Kubernetes |

### Fluxo do pipeline:

```yaml
stages:
  - build
  - deploy

build:
  stage: build
  image: docker:latest
  script:
    - docker build -t veras71/meusite-devops:$CI_COMMIT_SHORT_SHA .
    - docker push veras71/meusite-devops:$CI_COMMIT_SHORT_SHA

deploy:
  stage: deploy
  script:
    - kubectl set image deployment/meusite-devops meusite-devops=veras71/meusite-devops:$CI_COMMIT_SHORT_SHA
    - kubectl rollout status deployment/meusite-devops

📋 Pré-requisitos
Para rodar localmente:

    Docker (26.0+)

    Minikube (v1.35.0+)

    kubectl (1.28+)

    GitLab (19.2.0+)

    Helm (3.0+)

Variáveis de ambiente necessárias no GitLab:
Variável	Descrição
DOCKER_PASSWORD	Token de acesso do Docker Hub
KUBECONFIG	Configuração do cluster Kubernetes
🔧 Configuração do GitLab Runner

O GitLab Runner está configurado com executor Kubernetes, montando o socket do Docker:
yaml

[[runners.kubernetes.volumes.host_path]]
  name = "docker-sock"
  mount_path = "/var/run/docker.sock"
  host_path = "/var/run/docker.sock"

Permissões no Kubernetes:
bash

kubectl create clusterrolebinding gitlab-runner-admin \
  --clusterrole=cluster-admin \
  --serviceaccount=default:gitlab-runner

🌐 Como Acessar o Site

Após o deploy, o site fica disponível em:
bash

# IP do seu computador na rede (port-forward)
http://192.168.0.97:8080

# IP do Minikube (NodePort)
minikube service meusite-devops --url

🧪 Testando o Pipeline

    Faça uma alteração no código

    Commit e push para o GitLab:

bash

git add .
git commit -m "Minha nova feature"
git push origin main

    O pipeline será disparado automaticamente

    Acompanhe em CI/CD → Pipelines

📊 Monitoramento
Visualizar pods:
bash

kubectl get pods -w

Visualizar HPA:
bash

kubectl get hpa -w

Ver logs do runner:
bash

kubectl logs -l app=gitlab-runner

🛠️ Comandos Úteis
Gerenciamento do Minikube:
bash

minikube start          # Iniciar o cluster
minikube stop           # Parar o cluster
minikube status         # Verificar status
minikube dashboard      # Abrir o dashboard

Gerenciamento do Kubernetes:
bash

kubectl get pods        # Listar pods
kubectl get deployments # Listar deployments
kubectl get svc         # Listar serviços
kubectl logs <pod>      # Ver logs de um pod

Gerenciamento do GitLab Runner:
bash

helm install gitlab-runner gitlab/gitlab-runner -f values.yaml
kubectl logs -l app=gitlab-runner

🎯 Funcionalidades Implementadas

    ✅ CI/CD Automatizado com GitLab

    ✅ Build de Imagem Docker automático

    ✅ Push para Docker Hub automatizado

    ✅ Deploy no Kubernetes com zero downtime

    ✅ Horizontal Pod Autoscaler (HPA)

    ✅ Health Checks (Liveness e Readiness Probes)

    ✅ Redundância com 2-10 pods

    ✅ Load Balancer para acesso externo

    ✅ Dashboard no GitLab para monitoramento

🐛 Troubleshooting
Erro: Cannot connect to Docker daemon

Solução: Verificar se o socket do Docker está montado no pod.
Erro: User cannot get resource deployments

Solução: Aplicar permissões RBAC:
bash

kubectl create clusterrolebinding gitlab-runner-admin \
  --clusterrole=cluster-admin \
  --serviceaccount=default:gitlab-runner

Erro: file name too long

Solução: Verificar a formatação da variável KUBECONFIG (usar uma única linha).
📚 Referências

    GitLab CI/CD Documentation

    Kubernetes Documentation

    Docker Documentation

    Minikube Documentation

👨‍💻 Autor

Fabio Veras Machado

    GitLab: @FabioVerasMachado

    GitHub: @FabioVerasMachado

📄 Licença

Este projeto é para fins educacionais.
🙏 Agradecimentos

    DIO - Inspiração para o site "Mundo Invertido"

    GitLab - Plataforma de CI/CD

    Kubernetes - Orquestração de containers

    Docker - Containerização

🚀 Próximos Passos

    Integrar com ArgoCD para GitOps

    Adicionar monitoramento com Prometheus + Grafana

    Configurar GitLab Pages para documentação

    Implementar testes automatizados no pipeline

    Adicionar notificações no Slack

Feito com ❤️ e muito DevOps!