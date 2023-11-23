pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                  - name: gradle
                    image: gradle:5.4.1-jdk11-slim
                    command:
                    - cat
                    tty: true
                  - name: docker
                    image: docker:latest
                    command:
                    - cat
                    tty: true
                    volumeMounts:
                    - mountPath: /var/run/docker.sock
                      name: docker-sock
                  volumes:
                  - name: docker-sock
                    hostPath:
                      path: /var/run/docker.sock    
            '''
        }
    }
    stages {
        stage('Build-jar-file') {
            steps {
                container('gradle') {
                    sh './gradlew build'
                }
            }
        }
        stage('Build-Docker-image') {
            steps {
                container('docker') {
                    sh 'docker build -t mytest/testing-image:latest .'
                }
            }
        }
        stage('Run built image') {
            steps {
                container('docker') {
                    sh 'docker run -d -p 8888:8888 mytest/testing-image:latest'
                    // sh 'curl -v localhost:8888'
                }
            }
        }
    }
}