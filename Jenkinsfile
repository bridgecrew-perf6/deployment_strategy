pipeline {
	agent any
	environment{
		Docker_tag = getDockerTag()
	}
	stages{
	stage('Build docker image'){
		steps{
			sh "docker build . -t mohankrish3/app:${Docker_tag}"
		}
		}
		stage('Dockerhub push'){
		steps{
            withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhub-password', usernameVariable: 'dockerhub')]) {
				sh "docker login -u ${dockerhub}  -p ${dockerhub-password}"
				sh "docker push mohankrish3/app:${Docker_tag}"
            }
			}
		}

		}
	
	

	def getDockerTag(){
		def tag =sh script: 'git rev-parse HEAD', returnStdout: true
		return tag
	}
