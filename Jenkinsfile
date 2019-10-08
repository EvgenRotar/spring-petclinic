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

stage('Unit tests') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
    try {
      sh 'mvn verify'
    } catch(err) {
      junit 'target/surefire-reports/*.xml'
      throw err
    }
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
