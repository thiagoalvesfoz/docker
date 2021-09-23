# Trabalhando com Network

## tipos de network

| TIPOS     | O QUE ELE FAZ                                                         |
| ----------| --------------------------------------------------------------------- |
| `bridge`  | padrão. usado para comunicar container entre si                       |
| `host`    | mescla a network do docker com a network do host                      |
| `overlay` | utilizado para comunicar varios daemons do docker, utilizado com docker sworm   |
| `maclan`  | permite atribuir um endereço MAC a um container fazendo com que ele aparece como um dispositivo físico   |
| `none`    | desabilita toda a rede do container   |

[docker network doc ](https://docs.docker.com/network/)


para criar uma network basta digitar `docker network create minharede`
caso queira especifiar o tipo de rede utilize a flag `--driver <TIPO>`

```bash
$ docker network create --driver bridge minharede
```

## fazendo ping de container ao outro

```bash
$ docker network create --driver bridge redeubuntu bash  # cria a network
$ docker run -d -it --name ubuntu1 --network redeubuntu bash # imagem bash, iniciar no modo interativo
$ docker run -d -it --name ubuntu2 --network redeubuntu bash 
$ docker exec -it ubuntu1 bash
$ ping ubuntu2
```