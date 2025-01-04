pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/jefrya123/AZ_DevSecOps'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('cred') // Replace with your SonarQube token credentials ID
            }
            steps {
                withSonarQubeEnv('MySonarQube') { // Replace 'MySonarQube' with your SonarQube configuration name
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
