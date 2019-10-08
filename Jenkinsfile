pipeline {
  agent any
  stages {
    stage('Unit tests') {
      steps {
        sh 'mvn verify'
      }
    }

    stage('Sonar analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar \
               -Dsonar.projectKey=Petclinic \
               -Dsonar.host.url=http://localhost:9000 \
               -Dsonar.login=a7ac9a7ba12799c42cdb55b17bb813c360e367ce'
        }
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      junit 'target/surefire-reports/*.xml'
    }
  }
}