podTemplate(label: 'node-k8s', containers: [
    containerTemplate(name: 'node', image: 'node:8-alpine', ttyEnabled: true)    
  ],volumes: [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
    hostPathVolume(hostPath: '/usr/bin/docker', mountPath: '/usr/bin/docker')
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
            withDockerRegistry([credentialsId: 'dockerhub', url: https://registry.hub.docker.com']) {
                stage("build"){
                    app = docker.build "JenkinsTemplate"
                }
                stage ("publish"){
                    app.push 'master'
                }
            }
        }
    }
}