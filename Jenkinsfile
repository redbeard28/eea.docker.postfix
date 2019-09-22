/* Created by Jeremie CUADRADO
 Under GNU AFFERO GENERAL PUBLIC LICENSE
*/

pipeline {
    agent { label DOCKER_NODE }

    environment {
        branchVName = 'master'
        TAG = '0.2'
        IMAGE = 'redbeard28/eea_postfix'
    }

    stages{

        stage('Clone the GitHub repo'){

            steps{
                git url: "https://github.com/redbeard28/eea.docker.postfix.git", branch: "${branchVName}", credentialsId: "jenkins_github_pat"
            }
            post{
                success{
                    echo 'Successfuly clone your repo...'
                }
            }
        }
        stage('Build the Image...'){
            /*steps{
                timeout(time:5, unit:'MINUTES'){
                    input message:'Approuve Image Building'
                }
            }*/

            steps{
                script {
                    withDockerServer([uri: "tcp://${DOCKER_TCPIP}"]) {
                        /* login to the registry and push */
                        withDockerRegistry([credentialsId: 'DOCKERHUB', url: "https://index.docker.io/v1/"]) {
                            /* Prepare build command */
                            def image = docker.build("${IMAGE}:${TAG}")

                            image.push()

                        }
                    }
                }
            }
        }
    }
}
