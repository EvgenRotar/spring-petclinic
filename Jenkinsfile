stage('Unit tests') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
    try {
      sh 'mvn verify'
      junit 'target/surefire-reports/*.xml'
    } catch(err) {
      junit 'target/surefire-reports/*.xml'
      currentBuild.result = "FAILED"
      notifyFailed()
      throw err
    }
  }
}

stage('Sonar analysis') {
  node {
    try {
      withSonarQubeEnv('sonar') {
        env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
        sh 'mvn sonar:sonar \
             -Dsonar.projectKey=Petclinic \
             -Dsonar.host.url=http://localhost:9000 \
             -Dsonar.login=a7ac9a7ba12799c42cdb55b17bb813c360e367ce'
      }
      waitForQualityGate abortPipeline: true
    } catch (e) {
       currentBuild.result = "FAILED"
       notifyFailed()
       throw e
    }
  }
}

stage('Build') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
    try {
      sh 'mvn clean install -DskipTests'
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    } catch (e) {
       currentBuild.result = "FAILED"
       notifyFailed()
       throw e
    }
  }
}


 def notifyFailed() {
   emailext (
       subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
       body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
         <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
       recipientProviders: [[$class: 'DevelopersRecipientProvider']]
     )
 }