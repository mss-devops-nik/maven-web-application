node
{
    def mavenhome = tool name: "Maven 3.8.5"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([githubPush()])])
    stage ('checkoutcode')
    echo "Job name is : ${env.JOB_NAME}"
    echo "Branch name is : ${env.BRANCH_NAME}"
    echo "Node name is : ${env.NODE_NAME}"
    {
    git branch: 'development', credentialsId: 'a3e13267-aac5-40d7-973a-1bb63fc184a9', url: 'https://github.com/mss-devops-nik/maven-web-application/'
    }

    stage ('build')
    {
        sh "$mavenhome/bin/mvn clean package"
    }
    
    stage ('sonarqubereport')
    {
        sh "$mavenhome/bin/mvn sonar:sonar"
    }
    
    stage ('artifactrepo')
    {
        sh "$mavenhome/bin/mvn deploy"
    }
    
    stage ('deploytotomcarserver')
    {
        sshagent(['1be37b39-11dd-470f-8124-432736279306']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.36.168:/opt/apache-tomcat-9.0.63/webapps/"
    }
    }
    
}
