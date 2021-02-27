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

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
    }
//
    stage('Deploy to Test') {
	deploy adapters: [tomcat8(credentialsId: 'tomcat-1', path: '', url: 'http://52.255.157.89:8080/')], contextPath: '/QAWebapp', war: '**/*.war'
	//jiraSendDeploymentInfo environmentId: 'Test', environmentName: 'QA test', environmentType: 'testing', serviceIds: ['http://52.255.157.89:8080/QAWebapp/'], site: 'devopsbc.atlassian.net', state: 'successful'
    }
//
    stage('Store the Artifacts') {
        server.publishBuildInfo buildInfo
    }

 }
