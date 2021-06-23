node{
    
    def mavenhome = tool name: "maven3.6.3"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutCode'){
   git branch: 'development', credentialsId: '1326db46-fc1f-4681-b41b-03cfcc8cbd9e', url: 'https://github.com/SathishS31/maven-web-application.git'
 }
 
stage('Build'){
 sh "${mavenhome}/bin/mvn clean package"
 }
 
stage('Execute SonarQubeReport')
{
sh "${mavenhome}/bin/mvn sonar:sonar"
}

stage('UploadArctifactIntoNexus ')
{
sh "${mavenhome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer'){
    sshagent(['5b314819-a855-401f-8506-280528b60c41']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.131.128:/opt/apache-tomcat-9.0.46/webapps/"
    }
}

stage('SendEmailNotification'){
    emailext body: '''build is over...

Regards,
Sathish Subbiah,
9600440624''', subject: 'Build is over', to: 'sathishyohan7@gmail.com'
    
}
    
 
   
}
