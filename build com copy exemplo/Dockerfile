FROM nginx:latest
LABEL responsavel "Thales Vitor"

# criando o arquivo conteudo.html e adicionando seu conteúdo através do RUN
RUN echo "<h1>Sem conteúdo</h1>" > /usr/share/nginx/html/conteudo.html

# copiando qualquer arquivo .html da raís do projeto para a pasta apontada do container
COPY *.html /usr/share/nginx/html/