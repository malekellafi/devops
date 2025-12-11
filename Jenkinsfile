pipeline {
    agent any

    environment {
        SONAR_TOKEN = 'sqa_882fae06ee9791fa6494c70f13107f507c85ac74'
        IMAGE_NAME = 'my-app:latest'
        CONTAINER_NAME = 'my-app-container'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=my-project \
                      -Dsonar.host.url=http://192.168.33.10:9000 \
                      -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Construire l'image Docker
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Supprimer le conteneur existant si nécessaire
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    // Lancer le conteneur Docker
                    sh "docker run -d --name ${CONTAINER_NAME} -p 8085:8085 ${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé"
        }
    }
}
