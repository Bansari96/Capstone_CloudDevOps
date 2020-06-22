pipeline {
  agent any
  stages {
    stage('Create kubernetes cluster') {
      steps {
        withAWS(region: 'us-east-2', credentials: 'ecr_credentials') {
          sh '''
                  eksctl create cluster \
                  --name clustercapstone \
                  --version 1.13 \
                  --nodegroup-name standard \
                  --node-type t2.small \
                  --nodes 2 \
                  --nodes-min 1 \
                  --nodes-max 3 \
                  --node-ami auto \
                  --region us-east-2 \
                  --zones us-east-2a \
                  --zones us-east-2b \
                  --zones us-east-2c \
          '''
        }
      }
    }
  }
}
