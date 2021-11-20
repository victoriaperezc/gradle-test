pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "docker build -t victoriaperez/podinfo:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push victoriaperez/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Image') {
      steps {
        sh "docker rmi victoriaperez/podinfo:${env.BUILD_NUMBER}"
      }
    }
      stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
          //sh 'cat k8s_svc_deploy.yaml | sed "s//$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f k8s_svc_deploy.yaml'
        }
      }
  }
}
  }
