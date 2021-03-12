
node
{

def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '4', daysToKeepStr: '', numToKeepStr: '4')), pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')
{
git branch: 'development', credentialsId: '98cc2772-723c-401a-ad67-9d8f33110c93', url: 'https://github.com/aqeel0177/maven-web-application.git'
}

stage('Built')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployApp To TomcatServer')
{
sshagent(['f657aa8d-0ca6-4873-b9ee-490301804fcd']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.21.52.143:/opt/apache-tomcat-9.0.43/webapps/"
}
} 

}
