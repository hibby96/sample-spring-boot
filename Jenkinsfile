pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'chmod +x gradlew && ./gradlew clean build'
                //the same as mvn clean test package
            }
        }
        stage('sonarqube') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh './gradlew sonarqube'
                    sleep(10)
                }
            }
        }
        stage('sonarqube gatekeeper') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}