node 
{
    def mavenHome = tool name: "MVN3.8.1"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
  stage ('code checkout')
  {
      git branch: 'development', credentialsId: '910e28e0-3e29-4c47-bc6b-73ac85e144af', url: 'https://github.com/pavan7540/maven-web-application.git'
  }
  stage ('Build artifacts')
  {
      sh "${mavenHome}/bin/mvn clean package"
  }
  /*
  stage ('Execute SonarQube')
  {
      sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  stage ('Upload Artifacts to Nexus')
  {
      sh "${mavenHome}/bin/mvn deploy"
  }
  */
  stage ('Deploy application into Tomcat')
  {
   sshagent(['4cb30b4f-e9f0-4595-970e-570f5695d4ca']) {
    sh "scp -o StrictHostkeyChecking=no target/maven-web-application.war ec2-user@15.206.173.225:/opt/apache-tomcat-9.0.45/webapps"
}    
      
  }
  stage ('Email Notification')
  {
      emailext body: '''Build successful

Regards,
Pavan''', subject: 'Build status', to: 'pavan540.kumar@gmail.com'
  }
}
