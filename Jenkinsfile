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
            }
            stage('build') {
                sh 'npm install'
            }
            stage("build docker"){
					// https://issues.jenkins-ci.org/browse/JENKINS-46447
                    sh 'docker build -t natsukikana/jenkins_template .'
            }
            stage('push docker'){
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
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