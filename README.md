# Comandos Docker

Guia rápido de referência com os principais comandos Docker e Docker Compose.

## Índice

- [Como rodar o build e o container](#como-rodar-o-build-e-o-container)
- [Listar containers](#listar-containers)
- [Baixar imagens](#baixar-imagens)
- [Acessar terminal do container](#acessar-terminal-do-container)
- [Manipulação do container](#manipulação-do-container)
- [Log do container](#log-do-container)
- [Volumes](#volumes)
- [Docker Compose](#docker-compose)
- [Caso tenha dúvidas sobre os comandos?](#caso-tenha-duvidas-sobre-os-comandos?)

---

## Como rodar o build e o container

```bash
# Construir imagem a partir do Dockerfile (na mesma pasta)
docker build -t minha-imagem .

# Rodar o container no modo background (detached)
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
docker exec -it <nome-do-container> /bin/sh
# ou
docker exec -it <nome-do-container> /bin/bash

# Acessar o terminal de um novo container
docker container run -it debian bash
```

> ⚠️ **Atenção:** ao criar algo no container utilizando `run`, um novo container é criado
> a cada execução. Ou seja, as informações são perdidas a cada `run` executado, pois o
> container é recriado do zero.

```bash
# Executar comando no terminal sem abrir o terminal interativo
docker container exec <nome-do-container> <seu-comando>
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
docker container restart <nome do container>

# Desligar o container
docker container stop <nome do container>

# Iniciar um container já criado
docker container start <nome do container>
```

---

## Log do container

```bash
# Acessar o log
docker container logs <nome do container>

# Acessar dados detalhados do container
docker container inspect <nome do container>
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
docker-compose build

# 2) Subir os containers em background
docker compose up -d

# 3) Caso altere o Dockerfile ou o docker-compose.yml sobe o container já com a alteração aplicada no container
docker compose up --build

# 4) Derrubar os containers
docker compose down
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