pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'chmod +x gradlew && ./gradlew clean build'
                //the same as mvn test package
            }
        }
        stage('sonarqube') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh './gradlew sonarqube'
                    sleep(10)
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}