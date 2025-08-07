pipeline {
    agent any
    parameters {
        string name: 'VERSION', defaultValue: '4.0'
    }
    environment {
        DOCKERHUB_CRED = credentials ('dockerhub')
        IMAGEN = 'myapp-alpine'
    }
    stages {
        stage('Test de contenedores') {
            steps {
                sh 'docker info | grep "Name:"'  // Mostrará el nombre del contenedor dind
                sh 'hostname'                   // Mostrará el hostname del contenedor Jenkins
            }
        }
        stage('Docker build') {
            steps {
                sh "docker build -t ${DOCKERHUB_CRED_USR}/${env.IMAGEN}:${env.VERSION} ."
            }
        }
        stage('Log in a Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
            }
        }
        stage('Push de la imagen a Docker Hub') {
            steps {
                sh "docker push ${DOCKERHUB_CRED_USR}/${env.IMAGEN}:${env.VERSION}"
            }
        }
        stage('Docker run de mi contenedor') {
            steps {
                sh "if [ 'docker stop ${env.IMAGEN}' ] ; then docker rm -f ${env.IMAGEN} && docker run -d --name ${env.IMAGEN} -p 5000:5000 ${env.DOCKERHUB_CRED_USR}/${env.IMAGEN}:${env.VERSION} ; else docker run -d --name ${env.IMAGEN} -p 5000:5000 ${env.DOCKERHUB_CRED_USR}/${env.IMAGEN}:${env.VERSION} ; fi"
            }
        }
    }
}