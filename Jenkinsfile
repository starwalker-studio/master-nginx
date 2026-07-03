pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                echo 'Clonando repositorio...'
                checkout scm
            }
        }

        stage('Crear red compartida') {
            steps {
                echo 'Creando red proxy-network si no existe...'
                sh 'docker network create proxy-network || true'
            }
        }

        stage('Construir imagen') {
            steps {
                echo 'Construyendo imagen de Nginx master...'
                sh 'docker compose build'
            }
        }

        stage('Limpiar contenedor anterior') {
            steps {
                echo 'Bajando contenedor existente...'
                sh 'docker compose down'
            }
        }

        stage('Levantar contenedor') {
            steps {
                echo 'Levantando Nginx master...'
                sh 'docker compose up -d'
            }
        }

        stage('Verificar') {
            steps {
                echo 'Verificando...'
                sh 'docker ps'
                sh 'docker network inspect proxy-network'
            }
        }
    }

    post {
        success {
            echo 'Nginx master desplegado exitosamente'
        }
        failure {
            echo 'El deploy falló, revisa los logs'
        }
    }

    options {
       timestamps()
    }
}