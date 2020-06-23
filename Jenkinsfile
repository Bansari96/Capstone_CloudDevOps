pipeline {
        agent any
        stages {
                stage('Create kubernetes cluster') {
                        steps {
                                withAWS(region:'us-east-2', credentials:'ecr_credentials') {
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
                stage('Push Image') {
                        steps {
                        withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId: 'bansaripatel', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                                sh '''
                                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                                        docker push bansaripatel/capstoneproj_devops
                                '''
                        }
                        }
                }
                stage('Set current kubectl context') {
                        steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
                                        kubectl config use-context arn:aws:eks:us-east-2:627208585904:cluster/capstone-cluster
                                '''
                        }
                        }
                }
                stage('Deploying blue container') {
			steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
                                        kubectl apply -f ./blue-controller.json
                                '''
                                }
			}
		}

		stage('Deploying green container') {
			steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
                                        kubectl apply -f ./green-controller.json
                                '''
                        }
			}
		}

		stage('Running services for blue') {
			steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
                                        kubectl apply -f ./blue-service.json
                                '''
                        }
			}
		}
	
		stage('Running service for green') {
			steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
                                        kubectl apply -f ./green-service.json
                                '''
                        }
			}
		}

        }
}