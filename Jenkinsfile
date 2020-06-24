pipeline {
    agent any
        stages {
	    stage('Linting') {
		    steps {
			sh 'tidy -q -e *.html'
		    }
	    }
	    stage('Build Docker Image') {
		steps {
		    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'bansaripatel', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
			sh '''
			    docker build -t bansaripatel/capstoneproj:latest .
			'''
		    }
		}
	    }
		stage('Push Image') {
			steps {
			withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId: 'bansaripatel', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
				sh '''
					docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
					docker push bansaripatel/capstoneproj:latest
				'''
			}
			}
		}
                stage('Set current kubectl context') {
                        steps {
                        withAWS(region: 'us-east-2', credentials: 'bansariAWS') {
                                sh '''
					aws eks update-kubeconfig --name capstonecluster
                                        kubectl config use-context arn:aws:eks:us-east-2:627208585904:cluster/capstonecluster
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
