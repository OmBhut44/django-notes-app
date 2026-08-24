@Library('Shared') _
pipeline {
    agent { label "Alpha" }

    stages {

        stage("hello") {
            steps {
                script{
                    hello()
                }
            }
        }
        stage("Code") {
            steps {
                script {
                    clone("https://github.com/LondheShubham153/django-notes-app.git", "main")
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    docker_build("notes-app","latest","om4426")
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                script{
                    docker_push("notes-app", "latest", "om4426")
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "This is deploying the application"

                sh '''
                    docker-compose down
                    docker-compose up -d 
                '''

                echo "Deploy Successfully"
            }
        }

        stage("Test") {
            steps {
                echo "This is testing the application"

                sh '''
                    sleep 30
                    curl -f http://localhost:8000
                '''

                echo "Test Successfully"
            }
        }
    }

    post {
        always {
            sh "docker-compose ps"
        }
    }
}
