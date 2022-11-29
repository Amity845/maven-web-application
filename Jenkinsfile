node{
	
	def mavenHome = tool name: 'maven3.8.5'
	echo "The job name is ${env.JOB_NAME}"
	echo "The Build number is ${env.BUILD_NUMBER}"
	echo "The node name is ${env.NODE.NAME}"
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
	
	//Checkout Code state
	stage('CheckoutCode'){
	git branch: 'development', credentialsId: '3fbb53c3-f01b-4695-ba02-06a46329d14e', 
	url: 'https://github.com/Amity845/maven-web-application.git'	
	}
	
	//build
	stage('Build'){
	sh "${mavenHome} clean package"
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
}//node close
