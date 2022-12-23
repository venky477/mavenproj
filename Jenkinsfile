node("slave1") {
  stage("git") {
     checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/venky477/mavenproj.git']]])
  
  } 
  stage("maven") {
    sh 'mvn package'
  
  }
  stage("sonarqube") {

    sh '''mvn clean verify sonar:sonar \\
  -Dsonar.projectKey=script \\
  -Dsonar.host.url=http://100.27.6.103:9000 \\
  -Dsonar.login=sqp_91b3b0ccf893a3da2fdbe6577761a42b7c9e27c3'''
  
  }
  stage("nexus") {
   nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'webapp/target/webapp.war', type: 'war']], credentialsId: 'NEXUSCREDS', groupId: 'QA', nexusUrl: '3.227.16.91:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexusrelrepo', version: '$BUILD_ID'
  
  }
  stage("jfrog") {
   sh '''cp webapp/target/webapp.war webapp/target/webapp-$BUILD_ID.war
   curl -uadmin:AP5aY4iRmzKbEqDV49KP3TDNEQc -T webapp/target/webapp-$BUILD_ID.war "http://3.234.250.17:8081/artifactory/samplerepo/webapp-$BUILD_ID.war"
   rm -rf webapp/target/webapp-$BUILD_ID.war 
   '''
  
  }
  stage("tomcat") {
   deploy adapters: [tomcat9(credentialsId: 'tomcat_user', path: '', url: 'http://54.236.247.187:8080/')], contextPath: 'testscrt', war: '**/*.war'
  
  }
}

