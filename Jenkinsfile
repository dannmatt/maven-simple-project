pipeline {

    agent any

    environment {
        mvnHome = tool 'mvn3.5.2'
    }

    stages {
        stage('Preparation') { // for display purposes
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/dannmatt/maven-simple-project.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run the maven build
                    if (isUnix()) {
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                    } else {
                        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
                    }
                }
            }
        }

        stage('Deliver for staging') {
            when {
                branch 'staging'
            }
            steps {
                //sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }

        stage('Deploy for production') {
            when {
                branch 'master'
            }
            steps {
                //sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }
    }

    post {
        always {
            //archive "target/**/*"
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            junit 'target/surefire-reports/*.xml'
        }
    }
}