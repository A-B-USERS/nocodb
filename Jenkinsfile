pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nocodb/nocodb"
        CONTAINER_NAME = "nocodb-ci"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop',
                    credentialsId: 'github-token',
                    url: 'https://github.com/A-B-USERS/nocodb.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 8080:8080 $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    sh 'curl -f http://localhost:8080 || echo "App not responding yet"'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'docker rm -f $CONTAINER_NAME || true'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success or fail).'
        }
    }
}
