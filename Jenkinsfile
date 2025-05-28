def call(String) {
    node('cicd') {
        stage('Cloning') {
            checkout scm
        }

        stage('Containerization') {
            steps{
            withCredentials([usernamePassword(
                credentialsId: 'dochub-crd', 
                usernameVariable: 'DOCKER_USER', 
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh "docker build -t index ."
                sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                sh "docker push kadhir22/demo"
                sh "docker rmi kadhir22/demo"
            }
            }    
        }

        stage('Cleanup Workspace') {
            cleanWs()
        }

        stage('K8s Deployment') {
            sh "kubectl rollout restart deployment demo"
        }
    }
}
