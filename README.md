# ECS Release

Composite GitHub Action for handling the entire deployment flow to deploy a new Docker image to ECS. It combines the following steps:

* Configure AWS Credentials
* Login to ECR
* Build, tag, & push image to ECR
* Update image ID in ECS task definition
* Deploy task definition to ECS

## Example

```yml
name: Deploy Application

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}

      - name: Use Node.js 14
        uses: actions/setup-node@v1
        with:
          node-version: 14

      - name: Deploy application
        uses: elevatormedia/ecs-release-action@v1
        with:
          stage: production
          aws-access-key-id: 'ABC123abc'
          aws-secret-access-key: 'ABC123abcABC123abcABC123abc'
          aws-region: 'us-east-1'
          ecr-repository: 'my-app'
          image-tag: ${{ github.sha }}
          build-options: '--build-arg NODE_ENV=production'
          task-definition: ecs-task-def.json
          container-name: 'my-app-container'
          ecs-service-name: 'my-service'
          ecs-cluster-name: 'my-app-cluster'
```
