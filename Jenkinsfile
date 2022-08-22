node
{
  echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
  def mavenHome = tool name : "maven 3.6.3"
  stage('gitcheckout')
  {
  
  git credentialsId: '856393e9-dc07-4f82-bd72-ab08eb676292', url: 'https://github.com/jashuwass-eca/maven-web-application.git'
  
  }
  
  stage('build')
  {
    sh "${mavenHome}/bin/mvn clean package"
  
  }
  stage('ExecuteSonarQubeReport')
  {
    sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  stage('Upload package to nexus repository')
  {
    sh "${mavenHome}/bin/mvn deploy"
  }
  stage('deploy application to tomcat server')
  {
  sshagent(['826c34a8-80d1-4120-81a8-cf47acab4cd1']) 
    {
      sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.24.173:/opt/apache-tomcat-9.0.65/webapps/"
    }
  
  }

}
