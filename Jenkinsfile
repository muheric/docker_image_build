pipeline {
    environment {
        registry = "http://localhost:8081/repository/docker_hub/"
        registryCredential = 'docker_user'
        dockerImage = ''
    }
    agent any 
    stages {
        stage ('Cloning Git'){
            steps 
            {
                git 'https://github.com/muheric/dockerbuild_withjenkins.git'
            }
        }
        stage ('Building image') {
            steps {
                script 
                {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage ('Deploy Image')
        {
            steps {
                script {
                    docker.withRegistry('', registryCredential)
                    {
                        dockerImage.push()
                    }
                }
            }

        }

        stage ('Remove Unused docker image'){
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }

        }

    }

}

