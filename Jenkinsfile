node{

def mavenHome = tool name: "maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Build Number is: ${env.BUILD_NUMBER}"
echo "Build Display Name is: ${env.BUILD_DISPLAY_NAME}"
echo "Job Name is: ${env.JOB_NAME}"
echo "Node Name is:${env.NODE_NAME}"


stage('Checkout Code')
{
git branch: 'development', credentialsId: '6800f1a1-2245-40f0-8040-e84a4b88cd43', url: 'https://github.com/praveen-devops-batch/maven-web-application.git'
}


stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"

}

stage('Execute SonarQube Report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('Upload Artifact Into Nexus')
{
sh "${mavenHome}/bin/mvn deploy"
}



stage('Deploy App Into TomCat server')
{
sshagent(['38916e58-b1eb-4258-9024-acc988cdba35']) {
 
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.0.213:/opt/apache-tomcat-9.0.63/webapps"


}
}


}
