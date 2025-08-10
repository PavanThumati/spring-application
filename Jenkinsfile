@Library('my-jenkins-shared-library') _
pipeline{
	agent any
	tools {
        jdk 'JDK11'  // Use the name you configured
    }
	parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
    }
	stages{ 
		stage('gitcheckout'){
			when { expression {  params.action == 'create' } }
			steps{
				gitCheckOut(
					branch:'main', 
					url:'https://github.com/PavanThumati/spring-application.git'
				)
			}
		}

		stage('Unit Test maven'){
			when { expression {  params.action == 'create' } }
            steps{
               script{
                   mvnTest()
               }
            }
        }

		stage('Integration Test maven'){
			when { expression {  params.action == 'create' } }
            steps{
               script{
                   mvnIntegrationTest()
               }
            }
        }
		stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   def SonarQubecredentialsId = 'sonarqube-api'
                   staticCodeAnalysis(SonarQubecredentialsId)
               }
            }
        }

		stage('Quality Gate Status Check : Sonarqube'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonarqube-api'
                   QualityGateStatus(SonarQubecredentialsId)
               }
            }
        }


	}
}
