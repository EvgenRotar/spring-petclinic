stage('Unit tests') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
    try {
      sh 'mvn verify'
      junit 'target/surefire-reports/*.xml'
    } catch(err) {
      junit 'target/surefire-reports/*.xml'
      throw err
    }
  }
}

stage('Sonar analysis') {
  node {
    withSonarQubeEnv('sonar') {
      env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
      sh 'mvn sonar:sonar \
           -Dsonar.projectKey=Petclinic \
           -Dsonar.host.url=http://localhost:9000 \
           -Dsonar.login=a7ac9a7ba12799c42cdb55b17bb813c360e367ce'
    }
    waitForQualityGate abortPipeline: true
  }
}

stage('Build') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
    sh 'mvn clean install -DskipTests'
    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
  }
}

post {
         always {
             echo 'This will always run'
         }
         failure {
             mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "evgeny.rotar.br@gmail.com";
         }
}