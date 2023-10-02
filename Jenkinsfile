pipeline {
    agent {label 'jenkins-slave-label'}
    stages {
        stage('Run on Slave') {
            steps {
                script {
                    def dockerImage = 'public.ecr.aws/docker/library/maven:3.9-sapmachine'

                    // Run the Docker commands directly on the slave
                    sh "docker pull ${dockerImage}"
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace ${dockerImage} mvn --version"
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace ${dockerImage} git --version"

                    // You can continue with your other pipeline steps
                }
            }
        stage('Source') {
            steps {
                sh 'mvn --version'
                sh 'git --version'
                git branch: 'main',
                    url: 'https://github.com/elvislittle/ssh-agent-gpt.git'
            }
        }
        stage('Clean') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn clean'
                }
            }
        }
        stage('Test') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn test'
                }
            }
        }
        stage('Package') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn package -DskipTests'
                }
            }
        }
    }
}
