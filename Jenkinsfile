node{
	try{
	def mavenHome = tool name: 'maven3.8.5'
	echo "The job name is ${env.JOB_NAME}"
	echo "The Build number is ${env.BUILD_NUMBER}"
	echo "The node name is ${env.NODE_NAME}"
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
	
	//Checkout Code state
	stage('CheckoutCode'){
		sendSlackNotification("STARTED")
	git branch: 'development', credentialsId: '3fbb53c3-f01b-4695-ba02-06a46329d14e', 
	url: 'https://github.com/Amity845/maven-web-application.git'	
	}
	
	//build
	stage('Build'){
	sh "${mavenHome}/bin/mvn clean package"
	}
	/*
	//execute sonarQube Report
	stage('ExecuteSonarQubeReport'){
	sh "${mavenHome}/bin/mvn sonar:sonar"
	}
	
	//upload Artifact into nexus
	stage('UploadArtifactsIntoNexus'){
	sh "${mavenHome}/bin/mvn deploy"
	}
	
	//Deploy app into Tomcat Server
	stage('DeployApp'){
	//copied generated script from pipeline script
		sshagent(['450ed40d-3fe1-4f54-b544-2db977fe5956']) {
			//source and dest as tomcatServer user @ private IP
			sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war 
			ec2-user@172.31.39.68:/opt/apache-tomcat-9.0.68/webapps"
		}	
	}
	*/
	}//try clossing
	catch (e) {
    	// If there was an exception thrown, the build failed
   	 currentBuild.result = "FAILED"
   	 throw e
  	} 
 	 finally {
 	   // Success or failure, always send notifications
  	  sendSlackNotification(currentBuild.result)
  	}
	
}//node close



def sendSlackNotification(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
