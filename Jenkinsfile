node
{
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([githubPush()])])

def mavenHome = tool name: "Maven-3.6.3"
 
stage('Checkout code')
{
git branch: 'development', credentialsId: 'dd63d798-7137-4832-9d56-f13204e41a6f', url: 'https://github.com/devopswithdhana/maven-web-application-1.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('Execute sonaQube report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('Deploy to Tomcat')
{
sshagent(['My_tom']) 
{
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.5.184:/opt/apache-tomcat-9.0.55/webapps/"
}
stage('Send Email Notification')
{
mail bcc: 'makdhana04@gmail.com', body: '''Build  done

Regards,
Dhanasekar''', cc: 'makdhana04@gmail.com', from: '', replyTo: '', subject: 'Build over', to: 'makdhana04@gmail.com'
}
}
}
