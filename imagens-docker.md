
## primeiros comandos para testar

| COMANDO                       | O QUE ELE FAZ                                      |
| ----------------------------- | -------------------------------------------------- |
| `docker pull <imagename:tag>` | baixa uma imagem do Docker Hub (Docker Registry)   |
| `docker images`               | mostra todas as imagens baixadas                   |
| `docker rmi <imageName:tag>`  | remove uma imagem                                  |


# Criando a primeira imagem

```Dockerfile
FROM nginx:latest

RUN apt-get update
RUN apt-get install vim -y
```

Gerando uma nova imagem:  
`docker build -t thiagoalvesfoz/nginx-vim:latest .`

| COMANDO                         | O QUE ELE FAZ                                      |
| ------------------------------- | -------------------------------------------------- |
| `docker login`                  | faz o login no docker hub                          |
| `docker push <user/image:tag>`  | envia a sua imagem para o docker hub               |  

---

## Comandos Dockerfile

| COMANDO                       | O QUE ELE FAZ                                                       |
| ----------------------------- | ------------------------------------------------------------------- |
| `FROM`                        |    |
| `RUN`                         | executa um comando durante o processo de build                      |
| `WORKDIR`                     | define um diretório de trabalho                                     |
| `COPY`                        | copia um arquivo ou diretório para dentro da imagem                 |
| `CMD`                         | executa argumentos passados ao criar um container                   |
| `ENTRYPOINT`                  | define um comando fixo para executar sempre ao iniciar um container |


```Dockerfile
FROM ubuntu:latest

# define um comando fixo será executado quando inciar o container
ENTRYPOINT ["echo", "hello"]

# argumeto extra que pode ser subtituido ao iniciar o container
CMD ["world"]
```
saida: hello-world

exemplo 2
```Dockerfile
FROM nginx:latest

# copia da pasta html para o diretório dentro da imagem.
COPY html /usr/share/nginx/html

# recupera os entrypoint de inicialização
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon"]
```