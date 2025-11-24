pipeline {
  agent any

  options {
    ansiColor('xterm')
    timestamps()
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr: '20'))
    timeout(time: 60, unit: 'MINUTES')
  }

  parameters {
    string(name: 'GIT_REPO', defaultValue: 'https://github.com/your-org/your-repo.git', description: 'URL del repositorio GitHub (Spring Boot + Angular)')
    string(name: 'GIT_BRANCH', defaultValue: 'main', description: 'Rama a construir')
    string(name: 'FRONTEND_DIR', defaultValue: 'frontend', description: 'Ruta del proyecto Angular dentro del repo')
    string(name: 'BACKEND_DIR', defaultValue: 'backend', description: 'Ruta del proyecto Spring Boot dentro del repo')
    string(name: 'FRONTEND_BUILD_CMD', defaultValue: 'npm ci && npm run build -- --configuration=production', description: 'Comando de build para Angular')
    string(name: 'BACKEND_BUILD_CMD', defaultValue: 'mvn -B -DskipTests package', description: 'Comando de build para Spring Boot')
    string(name: 'DOCKER_REGISTRY', defaultValue: 'docker.io', description: 'Registro Docker (e.g. docker.io)')
    string(name: 'DOCKER_NAMESPACE', defaultValue: 'your-namespace', description: 'Namespace/usuario en el registro')
    string(name: 'IMAGE_FRONTEND', defaultValue: 'app-frontend', description: 'Nombre de la imagen del frontend')
    string(name: 'IMAGE_BACKEND', defaultValue: 'app-backend', description: 'Nombre de la imagen del backend')
    string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Tag de la imagen (se recomienda usar el commit)')
    booleanParam(name: 'PUSH_IMAGES', defaultValue: false, description: 'Empujar imágenes al registro Docker')
    booleanParam(name: 'DEPLOY_WITH_COMPOSE', defaultValue: false, description: 'Desplegar con docker-compose up -d')
    string(name: 'COMPOSE_FILE', defaultValue: 'docker-compose.yml', description: 'Ruta al docker-compose.yml')
  }

  environment {
    DOCKER_CREDENTIALS = credentials('docker-registry-creds')
    GIT_CREDENTIALS_ID = 'github-creds'
    // Ajusta las herramientas instaladas en Jenkins (Manage Jenkins > Global Tool Configuration)
    MAVEN_HOME = tool name: 'Maven3', type: 'maven'
    JDK_HOME = tool name: 'JDK11', type: 'jdk'
    NODEJS_HOME = tool name: 'Node16', type: 'nodejs'
    PATH = "${env.NODEJS_HOME}/bin:${env.MAVEN_HOME}/bin:${env.JDK_HOME}/bin:${env.PATH}"
  }

  stages {
    stage('Preparación') {
      steps {
        script {
          currentBuild.displayName = "#${env.BUILD_NUMBER} ${params.GIT_BRANCH} ${params.IMAGE_TAG}"
        }
        echo "Herramientas: Node=${NODEJS_HOME}, Maven=${MAVEN_HOME}, JDK=${JDK_HOME}"
      }
    }

    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: params.GIT_BRANCH]], userRemoteConfigs: [[url: params.GIT_REPO, credentialsId: env.GIT_CREDENTIALS_ID]]])
      }
    }

    stage('Build Frontend (Angular)') {
      steps {
        dir(params.FRONTEND_DIR) {
          sh "node -v && npm -v"
          sh "${params.FRONTEND_BUILD_CMD}"
        }
      }
    }

    stage('Build Backend (Spring Boot)') {
      steps {
        dir(params.BACKEND_DIR) {
          sh "${params.BACKEND_BUILD_CMD}"
        }
      }
      post {
        success {
          script {
            def jar = sh(script: "ls ${params.BACKEND_DIR}/*/target/*.jar || ls ${params.BACKEND_DIR}/target/*.jar", returnStdout: true).trim()
            echo "JAR compilado: ${jar}"
          }
        }
      }
    }

    stage('Docker Build') {
      steps {
        script {
          def registry = params.DOCKER_REGISTRY?.trim()
          def ns = params.DOCKER_NAMESPACE?.trim()
          env.FULL_FRONTEND_IMAGE = "${registry}/${ns}/${params.IMAGE_FRONTEND}:${params.IMAGE_TAG}"
          env.FULL_BACKEND_IMAGE = "${registry}/${ns}/${params.IMAGE_BACKEND}:${params.IMAGE_TAG}"
        }
        sh "docker version"
        dir(params.FRONTEND_DIR) {
          sh "docker build -t ${env.FULL_FRONTEND_IMAGE} -f Dockerfile ."
        }
        dir(params.BACKEND_DIR) {
          sh "docker build -t ${env.FULL_BACKEND_IMAGE} -f Dockerfile ."
        }
      }
    }

    stage('Docker Push (opcional)') {
      when { expression { return params.PUSH_IMAGES } }
      steps {
        withEnv(["DOCKER_CONFIG=${env.WORKSPACE}/.docker"]) {
          sh "echo ${DOCKER_CREDENTIALS_PSW} | docker login ${params.DOCKER_REGISTRY} -u ${DOCKER_CREDENTIALS_USR} --password-stdin"
          sh "docker push ${env.FULL_FRONTEND_IMAGE}"
          sh "docker push ${env.FULL_BACKEND_IMAGE}"
        }
      }
    }

    stage('Deploy con docker-compose (opcional)') {
      when { expression { return params.DEPLOY_WITH_COMPOSE } }
      steps {
        sh "docker compose version || docker-compose version"
        sh "COMPOSE_PROFILES=prod DOCKER_REGISTRY=${params.DOCKER_REGISTRY} DOCKER_NAMESPACE=${params.DOCKER_NAMESPACE} IMAGE_TAG=${params.IMAGE_TAG} docker compose -f ${params.COMPOSE_FILE} up -d || IMAGE_TAG=${params.IMAGE_TAG} docker-compose -f ${params.COMPOSE_FILE} up -d"
      }
    }
  }

  post {
    success { echo 'Pipeline OK' }
    failure { echo 'Pipeline falló' }
    always {
      cleanWs(deleteDirs: true, notFailBuild: true)
    }
  }
}
