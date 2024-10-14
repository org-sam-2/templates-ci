# Workflow Reutilizável de Build Java

Este workflow foi projetado para automatizar o processo de build de projetos Java.

## Como Usar

Para usar este workflow no seu projeto, você precisa definir um workflow do GitHub Actions e referenciar este workflow reutilizável. Abaixo está um exemplo:

### Exemplo de Workflow

```yaml
name: CI Pipeline

on:
  push:
    branches: ['main']

jobs:
  build:
    uses: org-sam-2/templates-ci/.github/workflows/build-java.yaml@main
    with:
      container_image: public.ecr.aws/docker/library/gradle:7.3.3-jdk17-alpine
      build_commands: |
        ./gradlew --version
        ./gradlew build
      artifact_paths: |
        build/
        arquivos.txt
