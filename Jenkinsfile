pipeline {
  agent {
    kubernetes {
      yamlFile 'jenkins/jenkins-pod.yaml'
    }

  }
  stages {
    stage('Build') {
      steps {
        container(name: 'kaniko') {
          sh "ls"
        }

      }
    }

    stage('Test') {
      steps {
        container(name: 'kustomize') {
          sh """
          set +e
          kubectl create namespace $PROJECT-${env.BRANCH_NAME.toLowerCase()}
          set -e
          """
          sh "kubectl run nginx --image=nginx -n $PROJECT-${env.BRANCH_NAME.toLowerCase()}"
          sh "kubectl get pods -n $PROJECT-${env.BRANCH_NAME.toLowerCase()}"
          sh "kubectl apply -f k3s.yaml -n $PROJECT-${env.BRANCH_NAME.toLowerCase()}"
          sh "./test.sh"
          sh "kubectl delete namespace $PROJECT-${env.BRANCH_NAME.toLowerCase()}"
        }

      }
    }
  }
  environment {
    PROJECT = 'jenkins-demo'
  }
}
