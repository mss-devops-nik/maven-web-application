node{
	try {
        notifyBuild('STARTED')
    	def mavenhome = tool name: "Maven 3.8.5"
    	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    	echo "Job name is : ${env.JOB_NAME}"
    	echo "Branch name is : ${env.BRANCH_NAME}"
   	echo "Node name is : ${env.NODE_NAME}"
    
    	stage ('checkoutcode') {
    		git branch: 'development', credentialsId: 'a3e13267-aac5-40d7-973a-1bb63fc184a9', url: 'https://github.com/mss-devops-nik/maven-web-application/'
    	}

    	stage ('build') {
       		sh "$mavenhome/bin/mvn clean package"
    	}
    
    	stage ('sonarqubereport') {
        	sh "$mavenhome/bin/mvn sonar:sonar"
    	}
    
    	stage ('artifactrepo') {
        	sh "$mavenhome/bin/mvn deploy"
    	}
    
   	stage ('deploytotomcarserver') {
        	sshagent(['1be37b39-11dd-470f-8124-432736279306']) {
        		sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.36.168:/opt/apache-tomcat-9.0.63/webapps/"
    		}
    	}
    
}
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
