pipeline { 
    agent any  
    stages { 
        
     stage ('Env') {
	steps {      
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "Artifcatory1"
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    
     // Tool name from Jenkins configuration
    
	rtMaven.tool = "maven"
	}
    }
	    
	    
        stage('Build') { 
            steps { 
               echo 'This is a minimal pipeline.' 
            }
        }
    }
}
