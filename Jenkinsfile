node
{
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
 def mavenHome = tool name: "maven3.6.2"
  stage('CheckoutCode')
  {
  git branch: 'development', credentialsId: '8c33368d-b31c-4d7c-ab5a-09cebf576f43', url: 'https://github.com/expandable-info/maven-web-application.git'
  }
  stage('Build')
  {
  sh "${mavenHome}/bin/mvn clean package"
  }
  stage('SonarQubeReport')
  {
  sh "${mavenHome}/bin/mvn sonar:sonar
  }
  stage('UploadArtifactNexus')
  {
  sh "${mavenHome}/bin/mvn deploy
  }
  stage('DeployApplication')
  {
  sshagent(['c6bb4429-7b15-4d8f-b7aa-606de39b0aca']) 
   {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.151.168:/opt/apache-tomcat-9.0.36/webapps/"
   }
  }
}
