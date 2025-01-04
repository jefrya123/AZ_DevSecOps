pipeline {
    agent { label 'ansible' }  // 'ansible' is your Jenkins agent label

    environment {
        SONARQUBE_URL = 'http://192.168.1.184:9000/'  // Your SonarQube server URL
        SONARQUBE_TOKEN = credentials('cred') // The credentials ID for SonarQube token
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('MySonarQube') {  // Using the SonarQube server configuration in Jenkins
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Waiting for SonarQube analysis result...'
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate()  // Wait for the SonarQube Quality Gate to pass/fail
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
