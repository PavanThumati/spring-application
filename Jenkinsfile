@Library('my-jenkins-shared-library') _
pipeline{
	agent any
	tools {
        jdk 'JDK11'  // Use the name you configured
    }
	stages{ 
		stage('gitcheckout'){
			steps{
				gitCheckOut(
					branch:'main', 
					url:'https://github.com/PavanThumati/spring-application.git'
				)
			}
		}

		stage('Unit Test maven'){
            steps{
               script{
                   mvnTest()
               }
            }
        }

		stage('Integration Test maven'){
            steps{
               script{
                   mvnIntegrationTest()
               }
            }
        }


	}
}
