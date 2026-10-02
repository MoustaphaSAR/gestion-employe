pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = 'tp1'
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
                    curl -fsS http://localhost:3000 > /dev/null
                    curl -fsS http://localhost:8000/docs > /dev/null
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
