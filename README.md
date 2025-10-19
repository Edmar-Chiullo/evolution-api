# README — Docker

Este diretório contém recursos e instruções para construir e executar a API (evolution-api) usando Docker.

## Visão geral
- Imagens e compose para executar a API em ambiente local ou em CI/CD.
- Presume-se que o Docker e, opcionalmente, Docker Compose estejam instalados.

## Pré-requisitos
- Docker Engine >= 20.x
- (Opcional) Docker Compose v2 ou compose integrado (docker compose ...)
- Variáveis de ambiente definidas em `.env` ou em sua ferramenta de orquestração

## Estrutura sugerida
- Dockerfile — define a imagem da API
- docker-compose.yml — serviços auxiliares (banco, cache) e a API
- .env — variáveis de configuração local

## Comandos úteis

Construir a imagem localmente:
```
docker build -t evolution-api:local .
```

Executar em primeiro plano:
```
docker run --rm -p 8080:8080 --env-file .env evolution-api:local
```

Executar com Docker Compose:
```
docker compose up --build
```

Executar em background:
```
docker compose up -d
```

Parar e remover containers/volumes criados pelo compose:
```
docker compose down --volumes
```

Listar logs (compose):
```
docker compose logs -f api
```

## Variáveis de ambiente recomendadas
- PORT (ex.: 8080)
- DATABASE_URL
- REDIS_URL
- NODE_ENV / ENVIRONMENT
Defina essas variáveis em `.env` e adicione `.env` ao `.gitignore` se contiver segredos.

## Volumes e dados persistentes
- Mapear diretórios de dados do banco para volumes Docker ou paths no host.
- Evitar mapear código em produção; use build de imagem.

## Boas práticas
- Minimizar a imagem final (multistage builds).
- Não incluir segredos diretamente no Dockerfile.
- Usar healthcheck para permitir orquestração confiável.
- Versionar a imagem com tags semânticas para deploys.

## Problemas comuns
- Permissões: ajustar usuário no Dockerfile ou mapear UID/GID correto.
- Portas ocupadas: verificar com `docker ps` e alterar mapeamento.
- Variáveis não carregadas: confirmar `.env` e `--env-file` ou compose env_file.

## Contato
- Documentar responsáveis e link para README do projeto (root) para instruções de desenvolvimento e deploy.
