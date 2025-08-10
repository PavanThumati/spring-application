@Library('my-jenkins-shared-library') _
pipeline{
	agent any
	tools {
        jdk 'JDK11'  // Use the name you configured
    }
	parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
		string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javaapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'pavanthumati')
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
		stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   mvnBuild()
               }
            }
        }
		stage('Docker Image Build'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
		stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
		stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }

	}
}
