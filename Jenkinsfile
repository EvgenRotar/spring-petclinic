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
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
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
