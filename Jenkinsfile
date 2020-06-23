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
                            docker build -t bansaripatel/capstoneproj_devops .
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
        }
}
