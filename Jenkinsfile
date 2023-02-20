pipeline {
  agent any
  // any, none, label, node, docker, dockerfile, kubernetes
  tools {
    maven 'my_maven'
  }
  environment {
    gitName = 'hig0ni'
    gitEmail = 'rjsgml658@naver.com'
    githubCredential = 'git_cre'
    dockerHubRegistry = 'hig0ni/sbimage'
  }
  stages {
    stage('Checkout Github') {
      steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: githubCredential, url: 'https://github.com/hig0ni/sb_code.git']]])
      }
      post {
        failure {
            echo 'Repository clone failure'
        }
        success {
            echo 'Repository clone success'
        }
      }
    }
    stage('Maven Build') {
      steps {
          sh 'mvn clean install'
      }
      post {
        failure {
            echo 'Maven jar build failure'
        }
        success {
            echo 'Maven jar build success'
        }
      }
    }
    stage('Docker Image Build') {
      steps {
          sh "docker build -t ${dockerHubRegistry}:${currentBuild.number} ."
      }
      post {
        failure {
            echo 'Docker image build failure'
        }
        success {
            echo 'Docker image build success'
        }
      }
    }
  }
}
