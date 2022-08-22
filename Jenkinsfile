node('nodes')
{

  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
  def mavenHome = tool name : "maven 3.6.3"
  stage('checkout the code')
  {
  
  git branch: 'development', credentialsId: '856393e9-dc07-4f82-bd72-ab08eb676292', url: 'https://github.com/jashuwass-eca/maven-web-application.git'
  
  }
  
  stage('Build')
  {
  
   sh "${mavenHome}/bin/mvn clean package"
   
   }
   stage('ExecuteSonarQubeReport')
   {
   
   sh "${mavenHome}/bin/mvn sonar:sonar"
   
   }
   stage('Upload the BuildArtifact into Nexus repository')
   {
   
   
   sh "${mavenHome}/bin/mvn deploy"
   
   }
   stage('deploy the application into tomcat server')
   {
      sshagent(['826c34a8-80d1-4120-81a8-cf47acab4cd1'])
   {
   
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.70.182:/opt/apache-tomcat-9.0.65/webapps/"
   }
   
   
   }
   stage('email notification')
   {
   
   emailext body: '''build is over

   Regards,
   Jashuwa VR.''', subject: 'build is over', to: 'krisjash999@gmail.com'
   }
  
}
