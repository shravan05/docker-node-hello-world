pipeline {
    agent any
    environment {
      DOCKER_TAG = getDockerTag()
      app_name = "nodejs"
    }
    stages {
        stage('Code Build') {
            steps {
               sh """
               cd ${env.WORKSPACE}
					     pwd
					     docker build -t nodejs-app .
                """
				       }
          }

        stage('Push Image to ECR') {
            steps {
                sh """

                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 926188890095.dkr.ecr.us-east-1.amazonaws.com
                docker tag nodejs-app:latest 926188890095.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:${DOCKER_TAG}
	             	docker push 926188890095.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:${DOCKER_TAG}

                 """

            }
        }

        stage('Checkout Helm Charts') {
            steps {

            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [],
            gitTool: 'GIT-Linux', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins-eks',
            url: 'https://github.com/shravan05/helm-eg.git']]])

            }
        }

        stage('Deploy Helm Charts') {
            steps {

                sh """
                    cd ${env.WORKSPACE}
                    ls -lrt


                """

            }
        }
    }
}


def getDockerTag() {
    def tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
    return tag
}
