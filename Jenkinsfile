pipeline {
    agent any
    tools {
        maven '3.8.5'
    }

    stages {
        stage('BUILD') {
            steps {
                echo 'Building..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'mvn --version'
                sh 'mvn -B -DskipTests clean package'
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
