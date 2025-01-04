pipeline {
    agent any
    environment {
        // Replace 'MySonarQube' with the name of your SonarQube server in Jenkins
        SONARQUBE_ENV = 'MySonarQube'
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add your build steps (e.g., Maven or Gradle)
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    echo 'Running SonarQube Analysis...'
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
