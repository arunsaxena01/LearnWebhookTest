node {
  
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page
    
    	def server = Artifactory.server "Artifcatory1"
    
    // Create an Artifactory Maven instance.
    
    	def rtMaven = Artifactory.newMavenBuild()
    	def buildInfo 
	
    // Tool name from Jenkins configuration
    
	rtMaven.tool = "Maven"
	
    // Set Artifactory repositories for dependencies resolution and artifacts deployment.
    
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
//  
	//slackSend channel: 'devopsbc', message: "started ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", tokenCredentialId: 'slack'
//	    
    stage('Clone source') {
        git url: 'https://github.com/arunsaxena01/DevOps-Demo-WebApp.git'
    }
//    

//    
     stage('SonarQube Analysis') {
        withSonarQubeEnv(credentialsId: 'akssonartoken', installationName: 'sonarqube') { // You can override the credential to be used
       		sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://52.152.224.93// -Dsonar.sources=. -Dsonar.tests=. -Dsonar.test.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.exclusions=**/test/java/servlet/createpage_junit.java'
        }
	timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
	    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
	    if (qg.status != 'OK') {
	      error "Pipeline aborted due to quality gate failure: ${qg.status}"
	    }
	}             
  }
    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
//


 }
