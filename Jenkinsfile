podTemplate(label: 'node-k8s', containers: [
    containerTemplate(name: 'node', image: 'node:8-alpine', ttyEnabled: true)    
  ]) {
    node('node-k8s') {
        app = container('node') {
            stage('Run Command') {
                print 'in side node'
                sh 'node --version'
                sh 'hostname -f'
            }

            stage('check out') {
                checkout scm
                sh 'npm install'
            }
            stage('Build - npm install') {
                dir ('app') {
                    sh 'npm install'
                }
            }
            stage("build docker"){
                    // https://issues.jenkins-ci.org/browse/JENKINS-46447
                    sh 'docker build -t natsukikana/jenkins_template .'
            }
            stage('push docker'){
                docker.withRegistry('https://nfdregistryacse51.azurecr.io', 'nfdregistryacse51') {
                    print "${BUILD_NUMBER}"
                    docker.image("natsukikana/jenkins_template").push("latest")
                    docker.image("natsukikana/jenkins_template").push("${BUILD_NUMBER}")
                    // sh 'docker login'
                    // sh 'docker push natsukikana/jenkins_template:latest'
                }
            }
        }
    }
}