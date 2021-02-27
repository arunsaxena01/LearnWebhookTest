pipeline {
stages {
    
    stage ('Env') {
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "Artifcatory1"
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    
     // Tool name from Jenkins configuration
    
	rtMaven.tool = "maven"
    }
	
	
    stage('Clone sources') {
	    steps{
        git url: 'https://github.com/arunsaxena01/DevOps-Demo-WebApp.git'
	    }
    }



    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}

post {
    always {
		 cleanWs()
      echo "Pipeline is done"
    }
  }
}
 
