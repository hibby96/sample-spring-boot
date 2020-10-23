pipeline {
    agent any
    stages {
        stage('compile') {
            steps {
                gradlew('clean', 'classes')
            }
        }
        stage('Unit Tests') {
            steps {
                gradlew('test')
            }
        }
    }
}