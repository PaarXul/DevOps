# Jenkins

Start typing here...

## Installation

Desde el navegador o terminal se debe descargar el repositorio del proyecto de Jenkins

```bash
git clone https://github.com/PaarXul/Jenkins-docker.git

git clone git@github.com:PaarXul/Jenkins-docker.git

```
Este repositorio contiene un archivo Dockerfile que se encarga de instalar Jenkins en un contenedor de Docker.

Contiene las siguientes instrucciones:

```BASH
docker context use defaul
docker build -t jenkins-cf:v4 .
```

```bash
sudo chown -R 1000:1000 /mount/var/jenkins_home -> se inicia en el build.sh

docker context use default
docker rm -f jblueQ

docker run -d --name jblueQ -p 8999:8080 -p 50000:50000 -v /mount/var/jenkins_home:/var/jenkins_home jenkins-cf:v4
```

