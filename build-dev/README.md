# Pressione CTRL + V para melhor visualização do conteúdo

### Crie a imagem(build) caso não a tenha no local onde se encontra o Dockerfile:
```sh
docker build -t nomeDaImagem .
```

### Comando para este contêiner rodar após build da imagem:
```sh
docker container run -it -v "$(pwd -W)":/app -p 80:8000 --name nomeDoContainerQueVoceQuer nomeDaImagemCriadaNoBuild
```

### Caso o contêiner já esteja criado apenas inicie o mesmo:
```sh
docker start -ai nomeDoContainer
```