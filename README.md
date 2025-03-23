# Pressione CTRL + V para melhor visualização do conteúdo

# Comandos Docker

## Baixar e rodar imagens

### Baixar e executar uma imagem de teste do Docker
```sh
docker container run hello-world
```

### Baixar uma imagem e rodar um container
```sh
docker container run debian
```

### Baixar uma imagem, rodar um container e executar um comando dentro dele
```sh
docker container run debian bash --version
```

## Listar containers

### Listar containers em execução
```sh
docker container ps
```

### Listar todos os containers (ativos ou parados)
```sh
docker container ps -a
```

## Remover containers

### Apagar um container específico
```sh
docker rm mydeb1
```

### Criar, rodar e apagar um container após a execução
```sh
docker container run --rm debian bash --version
```

## Gerenciar containers

### Criar um container com um nome específico
```sh
docker container run --name mydeb1 debian
```

### Parar um container em execução
```sh
docker container stop mydeb1
```
### iniciar container já criando com build e run(apenas iniciar o container)

```sh
docker start -ai nomeDoContainer
```

### Rodar um container sem que o serviço pare após execução (modo desacoplado)
```sh
docker run -dit --name mydeb1 debian bash
```

## Acessar o terminal de um container

### Acessar o terminal de um container já em execução
```sh
docker exec -it <container_name_or_id> bash
```

## Rodar um servidor web no Docker

### Rodar um container de servidor e expor uma porta
```sh
docker container run -d -p 8080:80 nginx
```
*(8080 é a porta exposta para a rede e 80 é a porta interna do container)*

## Como fazer o build(criar imagem personalizada com Dockerfile)

1) crie o arquivo Dockerfile
2) execute o comando que criará a imagem a partir do seu Dockerfile(o ponto final no fim do comando indica que o Dockerfile está no diretório em que você se encontra atualmente): 
```sh
docker image build -t nomeDaImagemPersonalizada .
```

3) Execute o seu container agora que você já possui a imagem personalizada criada:
```sh
docker container run -p 80:80 nomeDaImagemPersonalizada
```

## Flags de comando Docker

1) -p 80:8000 esta flag indica que a porta que será utilizada na máquina onde o contêiner foi iniciado é a porta 80 e a porta interna do contêiner escutada é 8000

