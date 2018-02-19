pipeline {
    agent any

    environment {
        mvnHome = tool 'mvn3.5.2'
    }

    parameters {
        string(defaultValue: "TEST", description: 'What environment?', name: 'userEnv')
        // choices are newline separated
        choice(choices: 'server_A\nserver_B', description: 'What Server?', name: 'server')
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
    }
    post {
        always {
            //archive "target/**/*"
            echo 'server: ${params.server}'
            echo 'userEnv: ${params.userEnv}'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            junit 'target/surefire-reports/*.xml'
        }
    }
}