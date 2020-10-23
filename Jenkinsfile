pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'chmod +x gradlew && ./gradlew clean build'
                //the same as mvn test package
            }
        }
    }
}