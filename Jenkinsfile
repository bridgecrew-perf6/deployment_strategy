pipeline {
        agent any
        environment{
                Docker_tag = getDockerTag()
        }
        stages{
        stage('Build docker image'){
                steps{
                        sh "docker build . -t dockerhub.kensium.com/millervet/app:${Docker_tag}"
                }
                }
                stage('Dockerhub push'){
                steps{
                    withCredentials([string(credentialsId: 'dockerhub_password', variable: 'dockerhub')]) {
                        sh "docker login dockerhub.kensium.com -u mohankrishnav -p $dockerhub"
                    }
                         sh "docker push dockerhub.kensium.com/millervet/app:${Docker_tag}"
                     }

                }
                stage('Run the Ansible playbook and initiate the new container'){
                        steps{
                                sh '''
                                       final_tag=$(echo $Docker_tag | tr -d ' ')
                                       sed -i "s/dockertag/$final_tag/g" docker-compose.yaml
                                       ansible-playbook ansible-playbook.yaml
                                '''
                        }
                }
				
    }

}
        def getDockerTag(){
                def tag =sh script: 'git rev-parse HEAD', returnStdout: true
                return tag
        }
