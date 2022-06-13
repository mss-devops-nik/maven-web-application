pipeline{
	agent any 
	tools{
		maven = "maven 3.8.4"
	}
	stages{
		stage('Checkoutcode'){
			step{
				git branch: 'development', credentialsId: 'a3e13267-aac5-40d7-973a-1bb63fc184a9', url: 'https://github.com/mss-devops-nik/maven-web-application/'
			}
		}
		stage('Build'){
			step{
				sh "mvn clean deploy"
			}
		}
		stage('SonarqubeReport'){
			step{
				sh "mvn sonar:sonar"
			}
		}
		stage('DeployBuildToNexus'){
			step{
				sh "mvn deploy"
			}
		}
		stage('TomcatDeploy'){
			step{
				sshagent(['1be37b39-11dd-470f-8124-432736279306']) {
					sh "scp target/maven-web-application.war ec2-user@172.31.36.168:/opt/apache-tomcat-9.0.63/webapps/"
				}
			}
		}
	}
}
