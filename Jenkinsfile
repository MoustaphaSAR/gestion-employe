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
                    wait_for_url() {
                        url="$1"
                        attempt=0
                        while [ "$attempt" -lt 30 ]; do
                            if curl -fsS "$url" > /dev/null; then
                                return 0
                            fi
                            attempt=$((attempt + 1))
                            sleep 2
                        done
                        echo "Timed out waiting for $url" >&2
                        return 1
                    }

                    wait_for_url http://host.docker.internal/
                    wait_for_url http://host.docker.internal/api/employes
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
