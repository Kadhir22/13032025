def call(String) {
node('cicd'){
    stage('cloning'){
        checkout scm
    }
    stage ("containerization") {
         withCredentials([usernamePassword(credentialsId: 'dochub-crd', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                sh "docker build -t index ."
                sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                sh "docker push kadhir22/demo"
                sh "docker rmi kadhir22/demo"
    }
    stage('Cleanup WorkSpace'){
        cleanWs()
    }
    stage ("K8s Deployment") {
        sh "kubectl rollout restart deployment demo"
    }
 }
}
