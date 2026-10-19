# 🧩 Jenkins - Automação e Integração Contínua

## 📘 O que é o Jenkins?

O **Jenkins** é uma ferramenta open source de **automação** focada em **Integração Contínua (CI)** e **Entrega Contínua (CD)**.  
Ele permite automatizar o processo de **build**, **teste** e **deploy** de aplicações, garantindo mais qualidade e agilidade no desenvolvimento de software.

Criado originalmente em Java, o Jenkins suporta **diversas linguagens** e **ferramentas de integração**, podendo ser facilmente estendido por meio de **plugins**.

---

## ⚙️ Principais Recursos

- 🏗️ **Automação de Builds:** compila, testa e empacota automaticamente seu código.
- 🧩 **Plugins:** mais de 1800 plugins disponíveis para integração com Git, Docker, Kubernetes, Maven, Gradle, SonarQube, etc.
- 🌀 **Pipelines:** definem o fluxo de CI/CD como código (`Jenkinsfile`).
- 🖥️ **Interface Web Intuitiva:** gerenciamento de jobs e monitoramento em tempo real.
- 🔐 **Integração com Controle de Acesso:** autenticação e autorização de usuários.
- ☁️ **Suporte a Nuvem:** integração com AWS, Azure, GCP e outras plataformas.

---

## 🚀 Benefícios do Jenkins

✅ Reduz falhas humanas no processo de deploy  
✅ Garante builds reprodutíveis e rastreáveis  
✅ Melhora a colaboração entre desenvolvedores e equipes DevOps  
✅ Detecta erros rapidamente com feedback contínuo  
✅ Permite escalar pipelines com agentes distribuídos

---

## 🧱 Estrutura Básica

Um pipeline no Jenkins é composto por **stages** e **steps**:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Compilando o código...'
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                echo 'Executando testes...'
                sh './gradlew test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Fazendo deploy da aplicação...'
                sh './deploy.sh'
            }
        }
    }
}
```

## 🐳 Exemplo com Docker

Você pode executar o Jenkins localmente com Docker:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

## 🔧 Integrações Comuns

| Ferramenta |	Função |
|-----------|------------|
| GitHub / GitLab |	Versionamento e Webhooks |
| Docker	|Empacotamento de aplicações |
| Kubernetes	| Deploy automatizado em clusters |
| SonarQube |	Análise de qualidade do código |
| Maven / Gradle |	Build e gestão de dependências |

## 📈 Pipeline de CI/CD Simplificado

1. Commit no GitHub