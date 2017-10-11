podTemplate(label: 'node-k8s', containers: [
    containerTemplate(name: 'node', image: 'node:8-alpine', ttyEnabled: true)    
  ]) {
    def app
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
        }
    }
    node{  
        stage('build container'){
            sh 'hostname -f'
            sh 'type docker'
            withDockerRegistry([credentialsId: 'nfdregistryacse51', url: 'https://nfdregistryacse51.azurecr.io']) {
                stage("build"){
                    app = docker.build "DTA-backend"
                }
                stage ("publish"){
                    app.push 'master'
                    app.push "${commit_id}"
                }
            }
        }
    }
}