stage('Build') {
  node {
    git url: 'https://github.com/EvgenRotar/spring-petclinic'
    env.PATH = "${tool 'MAVEN3.3.9'}/bin:${env.PATH}"
    sh 'mvn clean package'
    archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
  }
}

stage('Sonar analysis') {
  node {
    withSonarQubeEnv('sonar') {
      env.PATH = "${tool 'MAVEN3.3.9'}/bin:${env.PATH}"
      sh mvn sonar:sonar \
           -Dsonar.projectKey=Petclinic \
           -Dsonar.host.url=http://localhost:9000 \
           -Dsonar.login=a7ac9a7ba12799c42cdb55b17bb813c360e367ce
    }
  }
}