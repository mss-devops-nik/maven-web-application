pipeline{
	agent any 
	tools {
		maven "Maven 3.8.5"
	}
	triggers {
  		pollSCM '* * * * *'
	}
	options {
  		buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
  		timestamps()
	}
	stages{
		stage('Checkoutcode'){
			steps {
				git branch: 'development', credentialsId: 'a3e13267-aac5-40d7-973a-1bb63fc184a9', url: 'https://github.com/mss-devops-nik/maven-web-application/'
			}
		}
		stage('Build') {
			steps {
				sh "mvn clean package"
			}
		}
		stage('SonarqubeReport') {
			steps {
				sh "mvn sonar:sonar"
			}
		}
		stage('DeployBuildToNexus') {
			steps {
				sh "mvn deploy"
			}
		}
		stage('TomcatDeploy') {
			steps {
				sshagent(['1be37b39-11dd-470f-8124-432736279306']) {
					sh "scp target/maven-web-application.war ec2-user@172.31.36.168:/opt/apache-tomcat-9.0.63/webapps"
				}
			}
		}
	}
}
