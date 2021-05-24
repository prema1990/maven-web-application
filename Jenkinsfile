node 
{
def mvnhome = tool name: "Maven3.8.1"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutgitcode')
{
git branch: 'development', credentialsId: 'cb2bc8e5-0f87-4e6b-ad64-68cdc058fa2a', url: 'https://github.com/prema1990/maven-web-application.git'
}

stage('build')
{
sh "${mvnhome}/bin/mvn clean package"

}
stage('executesqreport')
{
sh "${mvnhome}/bin/mvn sonar:sonar"
}

stage('deployAItonexus')
{
sh "${mvnhome}/bin/mvn deploy"
}

stage('tomcatdeploy')
{
sshagent(['77a90337-64d0-4a03-9cc9-c5b37ddce63c']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.131.162.188:/opt/apache-tomcat-9.0.45/webapps/"

}
}

stage('sendemailnotification')
{
emailext body: '''buildover


by,
prema''', subject: 'buildmsg...', to: 'cpremu1990@gmail.com'

}
}
