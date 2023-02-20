pipeline {
  agent any
  // any, none, label, node, docker, dockerfile, kubernetes
  tools {
    maven 'my_maven'
  }
  environment {
    gitName = 'hig0ni'
    gitEmail = 'rjsgml658@naver.com'
  }
  stages {
    stage('Example') {
      steps {
        echo 'Hello World'
        }
    }
  }
}

