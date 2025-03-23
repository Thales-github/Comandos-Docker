FROM python:3.6
LABEL responsavel="Thales Vitor"

# && é utilizado para concatenar comandos, \ é utilizado para eu poder utilizar quebra de linha em comandos grandes
RUN useradd www && \
    mkdir /app && \
    mkdir /log && \
    chown www /log

# define usuário ativo ao executar contêiner
USER www

# criando volume do contêiner
VOLUME /log

# define deretório de trabalho
WORKDIR /app

# na aplicação a porta utilizada é '8000', então precisamos deixá-la exposta
EXPOSE 8000

# Aqui iremos apontar o caminho onde o python seja executado
ENTRYPOINT ["/usr/local/bin/python"]

# Executa o python na linha de comando
CMD ["run.py"]