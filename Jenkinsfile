pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.host.url=http://localhost:9000 \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.login=sqp_d41d37752220bb52497255bc9698ccbbdf1313bd'''
      }
    }

  }
}