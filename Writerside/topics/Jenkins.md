# Jenkins

## Instalación rápida con Docker
Desde el navegador o terminal se debe descargar el repositorio del proyecto de Jenkins (ejemplo):

```bash
git clone https://github.com/PaarXul/Jenkins-docker.git
# o con SSH
git clone git@github.com:PaarXul/Jenkins-docker.git
```
Este repositorio contiene un Dockerfile que instala Jenkins en un contenedor de Docker.

Instrucciones típicas:
```bash
docker context use default
docker build -t jenkins-cf:v4 .
```
Ejecutar el contenedor (asegura permisos correctos en la carpeta de datos):
```bash
sudo chown -R 1000:1000 /mount/var/jenkins_home  # también se ejecuta desde build.sh

docker context use default
docker rm -f jblueQ  # remover si existe

docker run -d --name jblueQ -p 8999:8080 -p 50000:50000 -v /mount/var/jenkins_home:/var/jenkins_home jenkins-cf:v4

# Si ves errores de permisos al crear carpetas:
sudo chown -R 1000:1000 /mount/var/jenkins_home
```

---

## CI/CD: Spring Boot + Angular en Docker con Jenkins + GitHub
Este proyecto incluye plantillas listas para usar:
- Jenkinsfile (en la raíz del repo).
- frontend/Dockerfile (Angular -> NGINX).
- backend/Dockerfile (Spring Boot -> JRE).
- docker-compose.yml (orquestación local o en servidor).

El pipeline realiza:
1) Checkout desde GitHub.
2) Build de Angular (npm ci + ng build).
3) Build de Spring Boot (mvn package).
4) Construcción de imágenes Docker frontend y backend.
5) (Opcional) Push de imágenes al registro Docker.
6) (Opcional) Despliegue con docker-compose.

### Requisitos en Jenkins
- Plugins:
  - Git, GitHub, Pipeline, Pipeline: Nodes and Processes, NodeJS, Docker, Docker Pipeline, Credentials Binding.
- Herramientas (Manage Jenkins > Global Tool Configuration):
  - JDK: JDK11 (o 17) con nombre "JDK11".
  - Maven: Maven3 con nombre "Maven3".
  - NodeJS: Node16 (o 18) con nombre "Node16" y opción "Install from nodejs.org".
- Agente con Docker instalado y en el grupo docker (si Jenkins corre en contenedor, suele bastar con Docker-in-Docker o exponer el socket).

### Credenciales requeridas
Crea en Jenkins (Manage Jenkins > Credentials):
- ID: github-creds
  - Tipo: Usuario y contraseña/token (HTTPS) o clave SSH. Usa el tipo que coincida con tu URL del repo.
- ID: docker-registry-creds
  - Tipo: Usuario y contraseña del registro Docker (Docker Hub u otro).

### Estructura esperada del repositorio
```
/ (raíz)
├─ Jenkinsfile
├─ docker-compose.yml
├─ frontend/
│  └─ Dockerfile  (Angular)
└─ backend/
   └─ Dockerfile  (Spring Boot)
```
Ajusta FRONTEND_DIR y BACKEND_DIR si tu estructura difiere.

### Crear el Job de Pipeline
1) New Item > Pipeline.
2) Marcar "This project is parameterized" si deseas cambiar parámetros en cada build.
3) Definition: Pipeline script from SCM > Git.
   - Repository URL: URL de tu repo (HTTPS o SSH).
   - Credentials: github-creds.
   - Branches to build: main (o la rama deseada).
   - Script Path: Jenkinsfile.
4) Guardar.

Alternativa: Multibranch Pipeline con el mismo Jenkinsfile en cada rama.

### Parámetros del Jenkinsfile
- GIT_REPO, GIT_BRANCH: ubicación del código.
- FRONTEND_DIR, BACKEND_DIR: rutas relativas de los proyectos.
- FRONTEND_BUILD_CMD, BACKEND_BUILD_CMD: comandos de build.
- DOCKER_REGISTRY, DOCKER_NAMESPACE: destino de las imágenes.
- IMAGE_FRONTEND, IMAGE_BACKEND, IMAGE_TAG: nombres y tag a publicar.
- PUSH_IMAGES (bool): publicar en el registro.
- DEPLOY_WITH_COMPOSE (bool): ejecutar docker compose up -d.
- COMPOSE_FILE: ruta del docker-compose.

### Webhook de GitHub (disparar builds automáticamente)
- En GitHub > Settings > Webhooks > Add webhook:
  - Payload URL: http(s)://TU-JENKINS/github-webhook/
  - Content type: application/json
  - Events: Just the push event (o según necesidad).
- En el Job: marca "Build when a change is pushed to GitHub" (requiere GitHub plugin).

### Despliegue con docker-compose
- Marca DEPLOY_WITH_COMPOSE = true.
- Asegura que el host tenga "docker compose" (o docker-compose). El pipeline intenta ambos.
- docker-compose.yml usa variables DOCKER_REGISTRY, DOCKER_NAMESPACE e IMAGE_TAG para apuntar a las imágenes construidas.

### Notas y consejos
- Angular: si tu build genera dist/<app-name> con nombre específico, ajusta el Dockerfile del frontend o pasa --build-arg APP_NAME.
- Spring Boot: si tu jar no queda como target/*.jar, ajusta el COPY del Dockerfile o el BACKEND_BUILD_CMD.
- Si tu Jenkins agente corre en Windows, adapta las etapas "sh" a "bat" o usa un agente Linux.


