pipeline {
  agent any

  stages {
    stage("Hello") {
      steps {
        echo "hello"

        // Script blocks can run any Groovy script
checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/arunsaxena01/DevOps-Demo-WebApp.git']]])
      }

    }
	
	    stage("Checkout") {
      steps {
        echo "hello"

        // Script blocks can run any Groovy script
        script {
          String res = env.MAKE_RESULT
          if (res != null) {
            echo "Setting build result ${res}"
            currentBuild.result = res
          } else {
            echo "All is well"
          }
        }
      }

    }
	
  }

  // All Stages and Pipeline can each have their own post section that is executed at different times
  post {
    always {
      echo "Pipeline is done"
    }
  }
}
