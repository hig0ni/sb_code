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
            echo 'Repository clone success'
        }
      }
    }
  }
}
