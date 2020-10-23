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
        stage('image build and push') {
            steps {
                script {
                   docker.withTool('docker') {
                        repoId = "hippy96/interview"
                        image = docker.build(repoId)
                        docker.withRegistry("https://registry.hub.docker.com", "mydockerhub") {
                            image.push()
                        }
                   }
                }
            }
        }
        stage('Deployment') {
            steps {
                    sh 'kubectl apply -f kubernetes.yml'
                    sh 'kubectl rollout restart deployment/sample-spring-boot'
            }
        }
    }
}
