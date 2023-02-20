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
    dockerHubRegistryCredential = 'docker_cre'
  }
  stages {
    stage('Checkout Github') {
      steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: githubCredential, url: 'https://github.com/hig0ni/sb_code.git']]])
      }
      post {
        failure {
            echo 'Repository Clone failure'
        }
        success {
            echo 'Repository Clone success'
        }
      }
    }
    stage('Maven Build') {
      steps {
          sh 'mvn clean install'
          // maven integration , maven invoker 플러그인이 설치되어있어야 함.
      }
      post {
        failure {
            echo 'Maven Jar Build failure'
        }
        success {
            echo 'Maven Jar Build success'
        }
      }
    }
    stage('Docker Image Build') {
      steps {
          sh "docker build -t ${dockerHubRegistry}:${currentBuild.number} ."
          sh "docker build -t ${dockerHubRegistry}:latest ."
          // currentBuild.number는 젠킨스가 제공하는 변수. 빌드넘버를 받아옴
          // dockerHubRegistry 위에서 선언한 변수, 내 저장소.
      }
      post {
        failure {
            echo 'Docker Image Build failure'
        }
        success {
            echo 'Docker Image Build success'
        }
      }
    }
    stage('Docker Image Push') {
      steps {
          // 도커 허브의 크리덴셜
          withDockerRegistry(credentialsId: dockerHubRegistryCredential, url: '') {
            // withDockerRegistry: docker pipeline 플러그인 설치시 사용가능
            // dockerHubRegistryCredential: environment에서 선언한 docker_cre
            sh "docker push ${dockerHubRegistry}:${currentBuild.number}"
            sh "docker push ${dockerHubRegistry}:latest"
          }  
      }
      post {
        //docker push가 성공하든 실패하든 로컬의 도커이미지는 삭제.
        failure {
          echo 'Docker Image Push failure'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
        }
        success {
          echo 'Docker Image Push success'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
        }
      }
    }
  }
}
