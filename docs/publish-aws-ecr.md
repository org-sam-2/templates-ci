# Workflow Reutilizável: Publicar no AWS ECR

Este repositório contém um workflow reutilizável do GitHub Actions para publicar imagens Docker no Amazon Elastic Container Registry (ECR).

## Como Usar

### Pré-requisitos

- **Conta AWS:** Certifique-se de ter uma conta AWS e configurado o [OICD (OpenID Connect)](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
 para usar IAM Role com permissão no AWS ECR.
- **Variáveis:** As variáveis `AWS_ACCOUNT_ID` e `AWS_REGION` devem ser configuradas a nível de repositório.

### Exemplo de Configuração

  Abaixo está um exemplo de como integrar o workflow reutilizável `publish-aws-ecr.yaml`:

  ```yaml
  name: CI/CD Pipeline

  on:
    push:
      branches: ['main']

  permissions:
    id-token: write

  jobs:
    publish:
      needs: [build]
      uses: org-sam-2/templates-ci/.github/workflows/publish-aws-ecr.yaml@main
      with:
        runs_on: ubuntu-latest
        environment: ''
        downloaded_name_artifacts: artifacts
        dockerfile: ./Dockerfile
        docker_build_context: .
        docker_push_ecr: true
        hadolint_no_fail: true
        aws_role_last_name: nome-da-role
        multi_platform: 'false'
