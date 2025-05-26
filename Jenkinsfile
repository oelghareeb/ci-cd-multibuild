@Library('session3')_

pipeline{
    agent {
        label 'agent1'
    }

    tools{
        jdk "java-8"
    }

    environment {
        DOCKER_USER = credentials('docker-user')
        DOCKER_PASS = credentials('docker-pass')
    }

    stages {
        stage('Login to DockerHub') {
            steps {
                script {
                    org.lab3.login(env.DOCKER_USER, env.DOCKER_PASS)
                }
            }
        }

        stage('Clone Repositories') {
            steps {
                script {
                    org.lab3.gitClone('https://github.com/oelghareeb/java.git', 'master', 'java')
                    org.lab3.gitClone('https://github.com/oelghareeb/python-CI-CD.git', 'main', 'python')
                }
            }
        }

        stage('Build and Push Docker Images') {
            parallel { // parallel to create and build the two images
                stage('Java Image') {
                    steps {
                        dir('java') {
                            script {
                                org.lab3.build('oelghareeb/java-app', 'latest')
                                org.lab3.push('oelghareeb/java-app', 'latest')
                            }
                        }
                    }
                }

                stage('Python Image') {
                    steps {
                        dir('python') {
                            script {
                                org.lab3.build('oelghareeb/python-app', 'latest')
                                org.lab3.push('oelghareeb/python-app', 'latest')
                            }
                        }
                    }
                }
            }
        }
    }
}
