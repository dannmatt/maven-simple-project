pipeline {
  agent any
  stages {
    stage('Preparation') {
      steps {
        git 'https://github.com/dannmatt/maven-simple-project.git'
      }
    }
    stage('Build') {
      steps {
        script {
          if (isUnix()) {
            sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
          } else {
            bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
          }
        }
        
      }
    }
    stage('Browser tests') {
      parallel {
        stage('Test IE') {
          steps {
            echo 'Testing IE'
          }
        }
        stage('Test Chrome') {
          steps {
            echo 'Testing Chrome'
          }
        }
        stage('Test Firefox') {
          steps {
            echo 'Testing Firefox'
          }
        }
        stage('Test Firefox on Unix') {
          steps {
            echo 'Testing Firefox on linux'
          }
        }
      }
    }
    stage('Deliver for staging') {
      when {
        branch 'staging'
      }
      steps {
        input 'Finished using the web site? (Click "Proceed" to continue)'
      }
    }
    stage('Deploy for production') {
      when {
        branch 'master'
      }
      steps {
        input 'Finished using the web site? (Click "Proceed" to continue)'
      }
    }
  }
  environment {
    mvnHome = 'mvn3.5.2'
  }
  post {
    always {
      archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      junit 'target/surefire-reports/*.xml'
      
    }
    
    success {
      echo 'This will run only if successful'
      
    }
    
    failure {
      echo 'This will run only if failed'
      
    }
    
    unstable {
      echo 'This will run only if the run was marked as unstable'
      
    }
    
    changed {
      echo 'This will run only if the state of the Pipeline has changed'
      echo 'For example, if the Pipeline was previously failing but is now successful'
      
    }
    
  }
}