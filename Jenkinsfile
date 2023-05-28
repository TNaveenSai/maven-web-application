node {
    def MavenHomeDir = tool name: "maven3.9.2"
    echo "Current Build Number: ${env.BUILD_NUMBER}"
    echo "Current Job Name: ${env.JOB_NAME}"
    echo "Current Node Name: ${env.NODE_NAME}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    stage('CHeckoutCode'){
                         git branch: 'development', credentialsId: '1759611c-3d4f-45e7-8628-fa588d1e7e2a', url: 'https://github.com/TNaveenSai/maven-web-application.git'
    }//GitClosure
    stage('Build'){
                 sh "$MavenHomeDir/bin/mvn clean package"
    }//MavenClosure
    stage('SonarQubeReport'){
                            sh "$MavenHomeDir/bin/mvn sonar:sonar"
    }//SonarqubeClosure
    stage('ArtifactToNexus'){
                            sh "$MavenHomeDir/bin/mvn deploy"
    }//NexusClosure
    stage('DeployAppToContainer'){
                                sshagent(['aceaef46-dcc4-47b6-8c56-1c38eea8ff6c']) {
                                                                                  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.19.202.166:/opt/apache-tomcat-9.0.75/webapps"
                                }//sshagentclosure
    }//AppDeployColsure
}//nodeClosure
