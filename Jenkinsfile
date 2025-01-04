pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning the Git repository...'
                git branch: 'main', url: 'https://github.com/jefrya123/AZ_DevSecOps'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Assuming a Maven project
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Replace 'cred' with your actual SonarQube token credentials ID in Jenkins
                SONAR_TOKEN = credentials('cred')
            }
            steps {
                echo 'Performing SonarQube analysis...'
                // Replace 'MySonarQube' with the name of your SonarQube server configured in Jenkins
                withSonarQubeEnv('MySonarQube') {
                    // Run the SonarQube analysis using Maven
                    sh 'mvn sonar:sonar -Dsonar.projectKey=AZ_DevSecOps -Dsonar.host.url=http://<sonarqube-url> -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Checking the Quality Gate...'
                // Use the 'waitForQualityGate' step to pause the pipeline until the analysis is complete
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
