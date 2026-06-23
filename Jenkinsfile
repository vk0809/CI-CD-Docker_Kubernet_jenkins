pipeline {
    agent any
    stages {
        stage ('clone') {
            steps {
                git 'https://github.com/vk0809/CI-CD-Docker_Kubernet_jenkins.git'
            }
        }
        stage ('build') {
            steps {
                sh 'docker build -t vk0908/flask-app:$(BUILD_NUMBER) .'
            }
        }
        stage ('push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-cred',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ])  {
                    sh '''
                    docker login -u $USER -p $PASS
                    docker push vk0908/flask-app:$(BUILD_NUMBER)
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl set image deployment/flask-app flask=vk0908/flask-app:${BUILD_NUMBER}
                '''
            }
        }
    }
}
