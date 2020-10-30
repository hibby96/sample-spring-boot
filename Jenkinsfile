pipeline {
    agent none
    stages {
        stage('build') {
            agent {
                docker { image 'gradle' }
            }
            steps {
                sh 'chmod +x gradlew && ./gradlew build'
            }
        }
        // stage('sonarqube') {
        //     agent {
        //         docker { image 'gradle' }
        //     }
        //     steps {
        //          withSonarQubeEnv('sonarcloud') {
        //              sh './gradlew sonarqube'
        //              sleep(10)
        //          }
        //      }
        //  }
        //  stage('sonarqube gatekeeper') {
        //      steps {
        //          timeout(time: 1, unit: 'HOURS') {
        //              waitForQualityGate abortPipeline: false
        //          }
        //      }
        //  }
        stage('docker build') {
            agent {
                docker { image 'docker' }
            }
            steps {
                sh "docker build -t hippy96/cog:${currentBuild.number} ."
                sh "docker tag hippy96/cog:${currentBuild.number} hippy96/cog:latest"
            }
        }
        stage('docker push') {
            agent {
                docker { image 'docker' }
            }
            steps {
                withDockerRegistry([credentialsId: 'creds-dockerhub', url: '']) {
                    sh "docker push hippy96/cog:${currentBuild.number}"
                    sh 'docker push hippy96/cog:latest'
                }
            }
        }
        stage('app deploy') {
            agent {
                docker { image 'alxl/kubectl' }
            }
            steps {
                withKubeConfig([credentialsId: 'kube-creds', serverUrl: 'https://kubernetes.docker.internal:6443']) {
                    sh 'kubectl apply -f kubernetes.yml'
                    sh 'kubectl rollout restart deployment/sample-spring-boot'
                }
            }
        }
    }
}