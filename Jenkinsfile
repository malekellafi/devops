pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('sonar-token')
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
                    mvn sonar: sonar \
                      -Dsonar.projectKey=my-project \
                      -Dsonar.host.url=http://192.168.33.10:9000 \
                      -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }
    }
}
