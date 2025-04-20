pipeline {
  agent {
    label 'workstation'
  }

  options {
    ansiColor('xterm')
  }

  parameters {
    string(name: 'env', defaultValue: '', description: 'Select Environment')
    string(name: 'component', defaultValue: '', description: 'Select Component')
    string(name: 'tag', defaultValue: '', description: 'Select Image Tag')
  }

  stages {
    stage('Update KubeConfig') {
      steps {
        sh 'aws eks update-kubeconfig --name ${env}-eks-cluster --region us-east-1'
      }
    }

    stage('Get APP Code') {
      steps {
        dir('APP') {
          git branch: 'main', url: 'https://github.com/Revanthsatyam/${component}'
        }
        dir('CHART') {
          git branch: 'main', url: 'https://github.com/Revanthsatyam/roboshop-helm'
        }
      }
    }

    stage('Deployment') {
      steps {
        sh 'helm upgrade --install ${component} ./CHART -f APP/helm/${env}.yaml --set image_tag=${tag}'
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}