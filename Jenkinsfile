pipeline {
    agent any
    environment {
      DOCKER_TAG = getDockerTag()
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
    }
}


def getDockerTag() {
    def tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
    return tag
}
