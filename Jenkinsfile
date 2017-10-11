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
				print app
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
            withDockerRegistry([credentialsId: 'dockerhub', url: 'https://registry.hub.docker.com']) {
                stage ("publish"){
                    app.push 'master'
                }
            }
        }
    }
}