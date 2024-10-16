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
```

### Lista de parâmetros

| Parâmetro               | Tipo    | Obrigatório | Valor Padrão     | Descrição                                                                                                                                                       |
|-------------------------|---------|-------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `runs_on`               | string  | Não         | ubuntu-latest    | Especifica o tipo de runner no qual o job será executado.                                                                                                        |
| `environment`           | string  | Não         | ""               | Define o contexto do ambiente para o job, como produção ou staging.                                                                                              |
| `downloaded_name_artifacts` | string  | Não         | artifacts         | Define o nome do diretório onde os artefatos serão baixados.                                                                                                     |
| `dockerfile`            | string  | Não         | ./Dockerfile      | Especifica o caminho para o Dockerfile usado para construir a imagem Docker.                                                                                     |
| `docker_build_context`  | string  | Não         | ./               | Define o diretório de contexto de build para o processo de build do Docker.                                                                                      |
| `docker_push_ecr`       | boolean | Não         | true             | Determina se a imagem Docker construída deve ser enviada para o AWS ECR.                                                                                         |
| `hadolint_no_fail`      | boolean | Não         | true             | Configura a ação Hadolint para falhar o workflow em erros de linting se definido como false.                                                                     |
| `aws_role_last_name`    | string  | Sim         | N/A              | Especifica o último nome da role IAM criada no formato `arn:aws:iam::AWS_ACCOUNT_ID:role/AWS_ROLE_LAST_NAME`.                                                    |
| `multi_platform`        | string  | Não         | false            | Indica se deve construir para múltiplas plataformas (linux/amd64, linux/arm64) quando definido como true.                                                        |
