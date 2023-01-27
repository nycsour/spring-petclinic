pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.host.url=http://10.10.10.37:9000 \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.login=sqp_d41d37752220bb52497255bc9698ccbbdf1313bd'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage(' Integration and Performance Tests') {
          steps {
            sh './mvnw verify'
            junit '**/target/surefire-reports'
          }
        }

      }
    }

  }
}