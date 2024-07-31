pipeline {
  environment {
    dockerimagename = "devopsbathinasirisha28/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/SirishaBathina/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub_credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Save the Kubernetes configuration to a file
                    writeFile file: 'kubeconfig', text: kubeconfig
                    // Set KUBECONFIG environment variable to point to the saved file
                    withEnv(["KUBECONFIG=${pwd()}/kubeconfig"]) {
                        sh '''
                            kubectl set image deployment=devopsbathinasirisha28/reactapp:latest
                            kubectl rollout status deployment
                        '''
                    }
                }
            }
        }
   /*
   stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", services: "service.yaml")

        }
      }
    }
  }
  */
}
