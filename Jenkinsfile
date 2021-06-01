
node ('wallmart-node')
{

 def mavenHome = tool name: "maven3.8.1"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 stage('CheckoutCode')
 {
 git branch: 'development', credentialsId: '079542fd-5432-4020-a748-c825197fc77a', url: 'https://github.com/mohan2905delight/maven-web-application.git'
 }

 stage('Build')
 {
 sh "${mavenHome}/bin/mvn clean package"
 }

 stage('ExecuteSonarQubeReport')
 {
 sh "${mavenHome}/bin/mvn sonar:sonar"
 }

 stage('UploadArtifactsintoNexus')
 {
 sh "${mavenHome}/bin/mvn deploy"
 }

 stage('DeployAppintoTomcatServer')
 {
 sshagent(['c9992cfd-52a6-4854-9e25-0cd0659168ba']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.32.207:/opt/apache-tomcat-9.0.45/webapps"
 }
 }

 stage('SendEmailNotification')
 {
 emailext body: '''Build Over

 Regards
 Mohankumar M S S DEVOPS''', subject: 'JENKINS Build Over', to: 'mohanms.msm@gmail.com'
 }

}
