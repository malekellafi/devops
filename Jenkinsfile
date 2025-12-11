pipeline {
    agent any

    environment {
        SONAR_TOKEN = 'sqa_882fae06ee9791fa6494c70f13107f507c85ac74'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
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
    }
}
