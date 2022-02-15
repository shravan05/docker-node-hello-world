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
                docker tag nodejs-app:latest 926188890095.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:${DOCKER_TAG}-${env.BUILD_NUMBER}
	             	docker push 926188890095.dkr.ecr.us-east-1.amazonaws.com/nodejs-app:${DOCKER_TAG}-${env.BUILD_NUMBER}

                 """

            }
        }
    }
}


def getDockerTag() {
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
