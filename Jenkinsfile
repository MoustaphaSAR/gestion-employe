pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'gestion-employe'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code source'
            }
        }

        stage('Build images Docker') {
            steps {
                sh 'docker compose build --pull'
            }
        }

        stage('Deploy application') {
            steps {
                sh 'docker compose up -d --remove-orphans'
            }
        }

        stage('Smoke test') {
            steps {
                sh '''
                    curl -fsS http://host.docker.internal/ > /dev/null
                    curl -fsS http://host.docker.internal/api/employes > /dev/null
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline Jenkins terminé.'
        }
        failure {
            sh 'docker compose logs --tail=100'
        }
    }
}
