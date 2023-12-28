pipeline {

agent none

triggers {pollSCM '* * * * *'}

parameters {
  choice choices: ['dev', 'qa'], name: 'Environment'
  string defaultValue: 'Punithkumar', name: 'name'
}

environment {
  env = "${params.Environment}"
  user = "${params.name}"
}

	stages {
  		stage('stage1') {
				agent { label 'new_agent' }
    				steps {
      					   echo "Hi This is ${user}"
					sh 'echo "Build URL: ${BUILD_URL}"'
    					}
  				}
		stage('stage2') {
				agent { label 'new_agent' }
    				steps {
      					echo "Deployment Environment: ${env}"
					sh 'echo "Jenkins URL: ${JENKINS_URL}"'
    					}
  				}

  		stage('stage3') {
				when {
				allOf {
                		expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
			
				}
            			}
				agent { label 'tomcat' }
    				steps {
      					sh 'echo "Branch Name: ${BRANCH_NAME}"'
    				}
  				}
    		stage('stage4') {
				when {
                		expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            			}
				agent { label 'tomcat' }
    				steps {
      					sh 'echo "Build ID: ${BUILD_ID}"'
    					}
				}
	
    	}
post {
    always {
        emailext(
            subject: "check the build",
            body: """
            Job: \${PROJECT_NAME}
            Build Number: \${BUILD_NUMBER}
            Build URL: \${BUILD_URL}
            User: \${BUILD_USER}
            """,
            to: "pgmadargaon@gmail.com",
        )
    }
}	
}
