pipeline {
    agent any
    environment {
        //be sure to replace "singhparul" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "singhparul/mydevopstest"
    }
    stages {
        
        
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    def registryCredential = 'dockerhub'
                    docker.withRegistry( '', registryCredential) {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }    
        }
        
         stage('Kubernetes Deploy') {
      steps {
        script {
          kubernetesDeploy(configs: "train-schedule-kube.yml", kubeconfigId: "kubeconfig",enableConfigSubstitution: true) 
        } 
      }
    }
        
        
        
    }
}
