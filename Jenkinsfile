pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'java8'
    }
    stages {
        stage('init') {
            steps {
                echo "This is initializing stage"
            }
        }

        stage('Build') {
            steps {
                echo "This is Build Stage"
                sh label: '', script: 'mvn clean package checkstyle:checkstyle'
            }
            post {
                success {
                    echo "Code Quality Check"
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                    echo "Publish Junit Report"
                    junit '**/surefire-reports/*.xml'
                    echo "Archive Artifact"
                    archiveArtifacts '**/webapp.war'
                }
            }
        }

        stage('Deploy') {
            steps {
                timeout(time: 90, unit: 'SECONDS') {
                    input 'Do you want to deploy in Dev Environment?'
            }
                echo "This is Deployment Stage"
                build 'dev-deploy'
            }
        }
    }
}