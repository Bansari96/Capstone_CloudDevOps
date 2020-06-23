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
        }
}
