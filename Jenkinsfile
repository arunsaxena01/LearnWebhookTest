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
	
      stage('UI Test') {
        buildInfo = rtMaven.run pom: 'functionaltest/pom.xml', goals: 'test'
	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'UI Test Report', reportTitles: ''])
    }
	
        stage('Deploy to Prod') {
	      deploy adapters: [tomcat8(credentialsId: 'tomcat-1', path: '', url: 'http://13.68.144.119:8080/')], contextPath: '/ProdWebapp', onFailure: false, war: '**/*.war'
	     //jiraSendDeploymentInfo environmentId: 'Staging', environmentName: 'Staging', environmentType: 'staging', serviceIds: ['http://13.68.144.119:8080/ProdWebapp'], site: 'devopsbc.atlassian.net', state: 'successful'
	     //jiraSendDeploymentInfo environmentId: 'Prod', environmentName: 'prod', environmentType: 'production', serviceIds: ['http://13.68.144.119:8080/ProdWebapp'], site: 'devopsbc.atlassian.net', state: 'successful'
         }
	
        stage('Sanity Test') {
        buildInfo = rtMaven.run pom: 'Acceptancetest/pom.xml', goals: 'test'
	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'Sanity Test Report', reportTitles: ''])
    }
 }
