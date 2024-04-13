pipeline {
    agent any
      tools {
        maven "3.9.6"
    } 
    stages {
        stage ("git cloning") {
            steps {
                git branch: 'main', url: 'https://github.com/qweciamoah/web-app.git'
            }
        }
        stage ( "build, test, package") {
            steps {
                sh '''
                mvn clean 
                mvn test
                mvn package 
                '''
            }
        }
        stage ("sonarqube  analysis") {
            environment {
                scannerHome= tool "sonarqube5.0.1"
            }
            steps {
                script{
                   withSonarQubeEnv(credentialsId: 'sonarQ-tk') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=webapp-CI"
                   }
                   
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}