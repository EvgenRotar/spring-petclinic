pipeline {
  agent any
  stages {
    stage('Unit tests') {
      steps {
        git url: 'https://github.com/EvgenRotar/spring-petclinic'
        env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
        sh 'mvn verify'
      }
    }

    stage('Sonar analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
          sh 'mvn sonar:sonar \
               -Dsonar.projectKey=Petclinic \
               -Dsonar.host.url=http://localhost:9000 \
               -Dsonar.login=a7ac9a7ba12799c42cdb55b17bb813c360e367ce'
        }
      }
    }

    stage('Build') {
      steps {
        git url: 'https://github.com/EvgenRotar/spring-petclinic'
        env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
        sh 'mvn clean install -DskipTests'
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
  post {
    always {
      junit 'target/surefire-reports/*.xml'
    }
  }
}