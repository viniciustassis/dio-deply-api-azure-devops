# Deploy de uma API Java na Nuvem com Azure
Para integrar o projeto de deploy de uma API Java com o Azure DevOps e gerenciar o pipeline usando o Visual Studio, você pode configurar o processo de CI/CD (Integração Contínua e Entrega Contínua).

## Passos
Passos para Usar Azure DevOps com Visual Studio

### 1. Criar um Repositório no Azure DevOps
- Acesse o Azure DevOps e crie um novo projeto.
- Adicione o código do projeto ao repositório, clonando o repositório remoto e fazendo o push com git push.

### 2. 2. Configurar o Pipeline de Build
- No portal Azure DevOps, acesse Pipelines > Criar Pipeline.
- Escolha GitHub ou o repositório no Azure DevOps.
- Selecione a linguagem de build como Maven (para projetos Java) e permita que o Azure gere o YAML básico:
```YAML
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: UseJavaVersion@1
    inputs:
      version: '11'
      architecture: 'x64'

  - script: mvn clean package
    displayName: 'Build with Maven'

  - publish: target/*.jar
    artifact: drop
```

### 3. Configurar o Pipeline de Release
- Em Releases, crie um novo pipeline de release.
- Configure o pipeline com as seguintes etapas:
    - Um artefato que será o resultado do pipeline de build.
    - Uma tarefa de deploy usando o Azure App Service:
        - Escolha Deploy Azure App Service.
        - Configure as credenciais da sua assinatura Azure.
        - Configure o tipo de aplicativo para Java.

### 4. Configurar o Serviço no Azure
- Crie um App Service para hospedar a API Java.
- Configure o plano de serviço e as dependências (por exemplo, o Azure Storage se necessário).