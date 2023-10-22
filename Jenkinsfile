pipeline {
  agent any
    
  tools {nodejs "nodejs"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubwithpassword', url: 'https://github.com/Ruksana92/Deploy-NodeJS-Helm-Chart-to-AWS-EKS-using-Jenkins-Pipeline.git']]])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t Ruksana92/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'ruksana92', variable: '')]) {
                    sh 'docker login -u Ruksana92 -p ${dockerhubpwd}'
                 }  
                 sh 'docker push Ruksana92/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App to helm chart on eks') {
      steps {
        script {
          kubernetesDeploy(configs: "nodeapp-deployment-service.yml", kubeconfigId: "kubernetes_config")
        }
      }
    }

  }
}
