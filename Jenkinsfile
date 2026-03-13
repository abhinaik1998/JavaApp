pipeline {
    agent any 

    tools {
        maven 'MAVEN'
        jdk 'JDK'
    }

    stages {

        stage('Fetch the code build') {
            steps {
                git branch: 'main', url: 'git@github.com:abhinaik1998/JavaApp.git'
            }
        }

        stage('Running unit tests build') {
            steps {
                dir('dock') {
                    bat 'mvn test'
                }
            }
        }

        stage('Running the install build') {
            steps {
                dir('dock') {
                    bat 'mvn install -DskipTests'
                }
            }
            post {
                success {
                    echo 'Build successful'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarServer') { // Server name in Configure System
                    dir('dock') {
                        bat 'sonar-scanner ' +
                            '-Dsonar.projectKey=JavaApp ' +
                            '-Dsonar.sources=src/main/java ' +
                            '-Dsonar.tests=src/test/java ' +
                            '-Dsonar.java.binaries=target/classes'
                    }
                }
            }
        }

    }

    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo 'All stages completed successfully'
        }
        failure {
            echo 'Pipeline failed! Check console output.'
        }
    }
}
