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
                  sh 'docker build -t ruksana92/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'ruksana92', variable: '')]) {
                    sh 'docker login -u ruksana92  -p  ${dockerhubpwd}'
                 }  
                 sh 'docker push ruksana92/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App to helm chart on eks') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name sample-ekscluster --region us-east-1')
          sh "kubectl get ns"
          sh "helm install nodeapp ./node-app"
        }
      }
    }

  }
}
