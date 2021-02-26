pipeline {
  agent any

  stages {
    stage("Checkout Project") {
      steps {

        // Script blocks can run any Groovy script
	checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, 
		  extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/arunsaxena01/DevOps-Demo-WebApp.git']]])
      }

    }
	
	stage("Build Project") {
            steps {
               // sh 'mvn -Dmaven.test.failure.ignore=true install' 
		  sh 'gradle clean compileJava test'
            }

    }
	

	
  }

  // All Stages and Pipeline can each have their own post section that is executed at different times
  post {
    always {
		 cleanWs()
      echo "Pipeline is done"
    }
  }
}
