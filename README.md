# Comandos Docker

Guia rápido de referência com os principais comandos Docker e Docker Compose.

OBS: Os nomes dos arquivos devem sempre ser 'Dockerfile' e 'docker-compose.yml'

## Índice

- [Como rodar o build e o container](#como-rodar-o-build-e-o-container)
- [Listar containers](#listar-containers)
- [Baixar imagens](#baixar-imagens)
- [Acessar terminal do container](#acessar-terminal-do-container)
- [Manipulação do container](#manipulação-do-container)
- [Log do container](#log-do-container)
- [Volumes](#volumes)
- [Docker Compose](#docker-compose)
- [Caso tenha dúvidas sobre os comandos](#caso-tenha-dúvidas-sobre-os-comandos)
- [Estratégias de reinicialização dos contêineres](#estratégias-de-reinicialização-dos-contêineres)
- [Exemplo de Dockerfile](#exemplo-de-dockerfile)
- [Exemplo docker-compose.yml com CONTROLE DE STARTUP](#exemplo-docker-compose.yml-com-controle-de-startup)
- [Exemplo docker-compose.yml rodando MIN-IO](#exemplo-docker-compose.yml-rodando-min-io)
- [Exemplo docker-compose.yml rodando Wordpress e Mysql com CONTROLE DE STARTUP](#exemplo-docker-compose.yml-rodando-wordpress-e-mysql-com-controle-de-startup)
---


## Como rodar o build e o container

```bash
# Construir imagem a partir do Dockerfile (na mesma pasta) e minha-imagem é o nome que você dará a imagem que será gerada a partir do Dockerfile
docker build -t minha-imagem .

# Rodar o container no modo background (detached) e minha-imagem é o nome que você dará a imagem que será gerada a partir do Dockerfile e meu-container o nome da aplicação(container) que vai ser iniciado no docker
docker run -d --name meu-container -p 8000:8000 minha-imagem

# Rodar o container e garantir que ele reinicie automaticamente
docker run -d --name meu-container --restart always -p 8000:8000 minha-imagem

# Verificar se o container está rodando
docker ps

# Ver os logs do container
docker logs -f meu-container
```

---

## Listar containers

```bash
# Listar containers rodando
docker container ls

# Listar todos os containers, independente se está rodando ou não
docker container ls -a
```

---

## Baixar imagens

```bash
# Listar imagens
docker image ls

# Baixar uma imagem e rodar o container
docker container run <nome da imagem>

# Baixar uma imagem, rodar o container e passar um comando
# para ser executado dentro do container em seguida
docker container run <nome da imagem> bash --version  # comando executado dentro do container
```

---

## Acessar terminal do container

```bash
# Acessar terminal do container
docker exec -it <NOME ou ID do container> /bin/sh
# ou
docker exec -it <NOME ou ID do container> /bin/bash

# Acessar o terminal de um novo container
docker container run -it debian bash
```

> ⚠️ **Atenção:** ao criar algo no container utilizando `run`, um novo container é criado
> a cada execução. Ou seja, as informações são perdidas a cada `run` executado, pois o
> container é recriado do zero.

```bash
# Executar comando no terminal sem abrir o terminal interativo
docker container exec <NOME ou ID do container> <seu-comando>
```

---

## Manipulação do container

```bash
# Criar container, rodar e apagar automaticamente após a execução
docker container run --rm debian bash --version

# Criar container com nome específico
docker container run --name mydeb1 debian

# Criar container com portas configuradas, rodando em background (-d)
# e com nome definido
docker container run --name nginx -p 8080:80 -d nginx

# Reiniciar o container (caso trave ou etc.)
docker container restart <NOME ou ID do container>

# Desligar o container
docker container stop <NOME ou ID do container>

# Iniciar um container já criado
docker container start <NOME ou ID do container>

# Apagar um container (Utilize o docker stop antes para parar o serviço do container antes de apagá-lo)
docker rm <NOME ou ID do container>

# Exibe as métricas de consumo do contêiner como RAM, CPU, REDE
docker container stats <NOME ou ID do container>
```

---

## Log do container

```bash
# Acessar o log
docker container logs <NOME ou ID do container>

# Acessar dados detalhados do container
docker container inspect <NOME ou ID do container>
```

---

## Volumes

```bash
# Listar volumes
docker volume ls
```

---

## Docker Compose

### O que é?

O Docker Compose serve para gerenciar (orquestrar) diversos contêineres. Dessa forma,
basta ter os `Dockerfile` de cada imagem necessária para construí-las e gerenciá-las
em conjunto.

### Passo a passo

```bash
# 1) Construir as imagens
docker compose build

# 2) Subir os containers em background
docker compose up -d

# 3) Caso altere o Dockerfile ou o docker-compose.yml sobe o container já com a alteração aplicada no container
docker compose up --build

# 4) Derrubar os containers
docker compose down

# 5) Verificar logs dos serviços do contêiner
docker compose logs

# 6) Listar serviços em execução
docker compose ps ou docker compose ls
```

## Estratégias de reinicialização dos contêineres

```bash
# O contêiner não será reiniciado automaticamente
no

# O contêiner sempre reiniciado automaticamente não importa o código de saída
always

# O contêiner será reiniciado somente se houver uma falha, código de saída != 0
on-failure

# 4) O contêiner será reiniciado automaticamente, a menos que ele seja parado explicitamente
unless-stopped

```

## Caso tenha dúvidas sobre os comandos?

```bash
# 1) Comandos de container
docker container --help

# 2) Comandos de imagem
docker image --help

# 3) Comandos de volume
docker volume --help

# 4) Comandos de rede
docker network --help

```

## Exemplo de Dockerfile

```bash

# Use uma imagem base do Python
FROM python:3.14-slim

#Defina o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copie os arquivos do projeto para o diretório de trabalho
COPY . /app

# Instale as dependências do projeto
RUN pip install -r requirements.txt

# Comando para executar a aplicação
CMD ["python", "python2.py"]

```

## Exemplo docker-compose.yml com CONTROLE DE STARTUP

```bash

services:
    web:
    build: .
    depends_on:
    db:
    condition: service_healthy
    restart: true
    redis:
    condition: service_started
redis:
    image: redis
db:
    image: postgres
healthcheck:
    test: [ curl -sSL , -X POST -H "Content-Type: application/json" -d '{ "password": "123456789" } ]
    interval: 10s
    retries: 5
    start_period: 30s
    timeout: 10s
```

## Exemplo docker-compose.yml rodando MIN-IO

```bash

#DOCKER COMPOSE MIN-IO

version: 1
services:
    minio:
    image: docker.io/bitnami/minio: 2022
    ports:
    - 9000:9000
    - 9001:9001
    networks:
    - minionetwork
    volumes:
    - /minio_data/data
    environment:
    - MINIO_ROOT_USER=admin
    - MINIO_ROOT_PASSWORD=minha_senha
    - MINIO_DEFAULT_BUCKETS=bucket-images
    networks:
    minionetwork:
    driver:bridge
    volumes:
    minio_data:
    driver:local

```

## Exemplo docker-compose.yml rodando Wordpress e Mysql com CONTROLE DE STARTUP

```bash

# Compose controle de startup

version:** '3.8'
services:
  - wordpress:
  - image: wordpress: 6.8
  - ports:
    - "8000:80"
- environment:
  - WORDPRESS_DB_HOST: db: 3306
  - WORDPRESS_DB_NAME: exampledb
  - WORDPRESS_DB_USER: examplexer
  - WORDPRESS_DB_PASSWORD: examplexpass
- volumes:
  - -wordpress_data:/var/www/html

- **db:**
  - image: mysql: 5.7
  - environment:
    - MYSQL_ROOT_PASSWORD: rootpass
    - MYSQL_DATABASE: exampledb
    - MYSQL_USER: examplexer
    - MYSQL_PASSWORD: examplexpass
- volumes:
  - -db_data:/var/lib/mysql

- **volumes:**
  - wordpress_data:
  - db_data:

```
