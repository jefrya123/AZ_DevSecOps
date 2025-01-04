pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/jefrya123/AZ_DevSecOps'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Replace 'sonar-token' with your Jenkins credential ID
            }
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh '''
                    mvn sonar:sonar \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
