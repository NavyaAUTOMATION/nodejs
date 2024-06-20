pipeline {
  agent any
  
   tools {nodejs "nodejs"}
    
  stages {
    stage("GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NavyaAUTOMATION/nodejs']])
                }
            }
        }
     
    stage('NodeJS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build NodeJS Docker Image') {
            steps {
                script {
                  sh 'docker build -t navya291195/nodeapp-3.1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'navya291195docker', variable: 'navya291195docker')]) {
                    sh 'docker login -u navya291195 -p ${navya291195docker}'
            }
            sh 'docker push navya291195/nodeapp-3.1.0'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name eks-demo-cluster1 --region us-east-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }

  }
}
