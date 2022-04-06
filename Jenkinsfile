pipeline {
    agent any
    tools {
        maven '3.8.5'
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "731727b0261c.mylabserver.com:8081"
        NEXUS_REPOSITORY = "maven-snapshots/"
        NEXUS_CREDENTIAL_ID = "Nexus"
      
    }

    stages {
        
        stage('SCAN & BUILD') {
            steps {
                echo 'Building..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'mvn --version'
               withSonarQubeEnv(installationName: 'SonarScanner', credentialsId: 'SonarToken') {
                sh 'mvn clean package sonar:sonar'
                }
            }
        }
        
        stage('TEST') {
            steps {
                echo 'Testing..'
                sh ' mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
            
        stage('DEPLOY') {
            steps {
                echo 'Deploying....'
                sh './jenkins/scripts/deliver.sh'
            }
        }
     }
  }
