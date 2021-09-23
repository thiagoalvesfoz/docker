
## Hello word

`docker run hello-world`

- **run**: Executa um container container a partir de uma imagem base. Primeiro ele busca a imagem localmente, caso não a encontre, irá buscar em um repositorio de imagens **(Docker Registry)** - **Docker Hub**.
- **hello-world**: É o nome da imagem solicitada, toda imagem possui uma tag de versão hello-world:TAG, caso essa tag seja omitida, por padrão a versão escolhida será a última (`hello-world:latest`)

## primeiros comandos para testar

| COMANDO                       | O QUE ELE FAZ                                      |
| ----------------------------- | -------------------------------------------------- |
| `docker run <imagename:tag>`  | executa um container a partir de uma imagem base   |
| `docker container ls`         | mostra os container em execução                    |
| `docker container ls -a`      | mostra todos os container incluindo os parados     |
| `docker ps`                   | mostra os container em execução                    |
| `docker ps -a`                | mostra todos os container incluindo os parados     |


## Executando Ubuntu

comando: `docker run -it ubuntu`

- **-it**: Diz ao Docker que ele deve abrir a instância do contêiner no modo interativo.
- **bash**: comando que será executado dentro do container.

| COMANDO                           | O QUE ELE FAZ                                                    |
| --------------------------------- | ---------------------------------------------------------------- |
| `docker run -it <imagename:tag>`  | inicia um container e logo em seguida entra no modo interativo   |
| `docker run --rm -it <image:tag>` | inicia um container, entra no modo interativo e após finalizar o processo, o container é removido                |

## Publicando portas

comando: `docker run --rm -p 8080:80 nginx`  

- **-p**: flag que diz ao docke que o container deve publicar uma porta para ser acessado.  
  - `8080:80`: o docker irá redirecionar a porta **8080** da máquina local para a porta **80** dentro do container.

| COMANDO                          | O QUE ELE FAZ                                                         |
| -------------------------------- | --------------------------------------------------------------------- |
| `docker stop <ID-CONTAINER>`     | para um container em execução                                         |
| `docker start <ID-CONTAINER>`    | iniciar um container parado                                           |
| `docker run -d <imagename:tag>`  | inicia um container desatachado o processo do container do terminal   |

## Removendo containers

| COMANDO                                | O QUE ELE FAZ                                          |
| -------------------------------------- | ------------------------------------------------------ |
| `docker rm <ID-OU-NOME-CONTAINER>`     | remove um container                                    |
| `docker rm <ID-OU-NOME-CONTAINER> -f`  | força a remoção de um container (caso o container esteja em execução)                                      |

## Acessando e alterando arquivos em um container

para entrar dentro de um container que já está execução, utiliza-se o comando `exec` para indicar
que deseja executar um comando, seguido da flag `-it` para indicar que deseja manter o terminal dentro do container no modo interativo.

Ex:  
```bash
$ docker run -d --name nginx -p 8080:80 nginx
$ docker exec -it nginx bash
```  
- tente editar a página padrão do nginx dentro do container - diretório > `/usr/share/nginx/html/`
  - utilize o vim para editar o index.html
    - instale com: `apg-get update && apt-get install vim`



## bind mounts

Os container são imutáveis, ou seja, qualquer operação de escrita e gravação em um container será perdido após o seu processo ser interrompido, para contornar isso utilizamos pontos de montagens em que compartilhamos um local da nossa máquina com o container, assim toda operação de escrita feita dentro de um container pode ser persistido, pois os arquivos são salvos dentro da sua máquina local e não no container.


```bash
$ docker run -d --name nginx -p 8080:80 -v ~/docker/html/:/usr/share/nginx/html/ nginx
$ docker run -d --name nginx -p 8080:80 -v $(pwd)/html/:/usr/share/nginx/html/ nginx
```  
**pwd**: comando shell para exibir o caminho completo do seu diretório atual.

| flag                                           | O QUE ELE FAZ                                          |
| ---------------------------------------------- | ------------------------------------------------------ |
| `-v <seu-diretorio>:<diretorio-no-container>`  | mapeia um diretório do seu computador para um diretório dentro do container | 
| `--mout type=bind,source="$(pwd)/html,target=/usr/share/nginx/html"`  | mapeia um diretório do seu computador para um diretório dentro do container |   

*o que acontece se você apontar para um diretório que não existe?* 


## Compartilhando volumes entre containers

```bash
$ docker volume create meuvolume
$ docker run -d --name nginx  --mount type=volume,source=meuvolume,target=/app nginx
$ docker run -d --name nginx2 --mount type=volume,source=meuvolume,target=/app nginx
$ docker run -d --name nginx3 -v meuvolume:/app nginx
```

para remover volumes não utilizados  
`docker volume prune`

