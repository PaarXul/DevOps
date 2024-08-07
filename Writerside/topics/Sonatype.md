# Sonatype

Start typing here...

## Installation

Desde el navegador o terminal se debe descargar el repositorio del proyecto de Sonatype

```bash

# Segun la documentacion de docker hub
$ docker volume create --name nexus-data
$ docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3

# Obtener la contraseña de administrador
$ docker exec -it nexus cat /nexus-data/admin.password

```
