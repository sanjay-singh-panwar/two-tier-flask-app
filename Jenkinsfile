pipeline {
    agent any

    stages {

        stage("Code") {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sanjay-singh-panwar/two-tier-flask-app.git'
            }
        }

        stage("Build Image") {
            steps {
                sh '''
                #!/bin/bash
                docker build -t two-tier-app .
                '''
            }
        }

        stage("Test") {
            steps {
                echo "Code testing stage (placeholder)"
            }
        }

        stage("Image Push") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh '''
                    #!/bin/bash
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag two-tier-app $DOCKER_USER/two-tier-app:latest
                    docker push $DOCKER_USER/two-tier-app:latest
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                sh '''
                #!/bin/bash
                docker compose down -v --remove-orphans || true
                docker compose up -d --build
                '''
            }
        }
    }
}

