pipeline {
    agent { label 'cicd' }

    stages {
        stage('Cloning') {
            steps {
                checkout scm
            }
        }

        stage('Containerization') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dochub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'docker build -t index .'
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push kadhir22/demo'
                    sh 'docker rmi kadhir22/demo'
                }
            }
        }

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('K8s Deployment') {
            steps {
                sh 'kubectl rollout restart deployment demo'
            }
        }
    }
}
