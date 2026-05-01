pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // En un entorno CI real se debe apuntar a una DB de test, aquí usamos la configuración normal.
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build & Deploy') {
            steps {
                script {
                    echo 'Building and restarting Docker container...'
                    // Esto asume que Jenkins corre con permisos de Docker y el docker-compose.yml está presente
                    // sh 'docker-compose up -d --build app'
                }
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo 'Pipeline completado con éxito.'
        }
        failure {
            echo 'Pipeline falló. Revisar logs.'
        }
    }
}
