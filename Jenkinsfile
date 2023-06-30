pipeline {  
    environment {
      registry = "cartaxovc/mobead_image_build"
      registryCredential = 'docker-user'
      dockerImage = ''
    }
    agent any 
    stages { 
        stage('Lint Dockerfile'){ 
            steps{
                echo "Pipeline Usando Jenkinsfile"
                sh 'docker run --rm -i hadolint/hadolint < Dockerfile'
            }
        }
        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Delivery image') {
            steps{
                script {
                  docker.withRegistry('https://registry-1.docker.io/v2/', 'docker-user') {
                   dockerImage.push("$BUILD_NUMBER")
                  }
                }
            }
        }
        stage('Delivery image') {
            steps{
                script {
                  sh 'docker run -p -d 81:80 --name=mobead_image_build' + dockerImage
                }
            }
        }

    } 
}