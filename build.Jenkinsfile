pipeline {
    agent {
        docker {
            label 'general'
            image '933060838752.dkr.ecr.eu-north-1.amazonaws.com/alonit_jenkins_agent:0.1'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        ECR_URL = '933060838752.dkr.ecr.eu-north-1.amazonaws.com'
        IMAGE_NAME = 'alonit-roberta'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin $ECR_URL
                docker build -t alonit-roberta:$BUILD_NUMBER .
                docker tag alonit-roberta:$BUILD_NUMBER $ECR_URL/$IMAGE_NAME:$BUILD_NUMBER
                docker push $ECR_URL/$IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'RobertaDeploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "$ECR_URL/$IMAGE_NAME:$BUILD_NUMBER")
                ]
            }
        }

    }
}
