pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        echo '1.Prepare Stage'
        script {
          build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
          if (env.BRANCH_NAME != 'main') {
            build_tag = "${env.BRANCH_NAME}-${build_tag}"
            //    build_tag = "${BUILD_TAG}-${build_tag}"
          }
        }

      }
    }

    stage('build') {
      steps {
        sh 'docker build -t rockstics/jenkins-demo:${build_tag} .'
      }
    }

    stage('push') {
      steps {
        sh 'docker push rockstics/jenkins-demo:${build_tag}'
      }
    }

    stage('deploy') {
      steps {
        sh 'kubectl apply -f k8s.yaml --record'
      }
    }

  }
}