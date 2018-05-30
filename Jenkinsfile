pipeline {

  agent none

  environment {
     CONTINUE_EXECUTION = 'true'
  }

  stages {

    stage('build') {
      agent {
        docker {
          image 'hseeberger/scala-sbt'
          args '-v docker-sbt-home:/docker-java-home'
        }
      }
      steps {
        sh 'ls'//'sbt compile'
      }
    }

    stage('codeCoverage') {
     agent {
        docker {
          image 'hseeberger/scala-sbt'
          args '-v docker-sbt-home:/docker-java-home'
        }
     }
    //launch coverage test
      steps {

         echo "currentBuild status: ${currentBuild.result}"
         try {
            sh 'sbt clean coverage test coverageReport'
         }
         catch (exc) {
             echo 'Something failed, I should sound the klaxons!'
             echo "currentBuild status: ${currentBuild.result}"
                          echo "currentBuild status: ${error}"
         }

         script{
                echo binding.variables
               if (binding.variables.containsKey('CONTINUE_EXECUTION')) {
                   echo "la libreria funziona"
               }


            env.CONTINUE_EXECUTION = "FUNZIONA"
            echo env.CONTINUE_EXECUTION
         }
      }

    }


    stage('postCodeCoverage') {

      input {
        message 'coverage test not passed, do you want to continue anyway?'
        id 'Yes'
        parameters {
          string(name: 'CONTINUE', defaultValue: 'false')
        }
      }
      steps {
        script {
                     echo "Continue execution: ${env.CONTINUE_EXECUTION}"
                     echo "Continue execution: ${CONTINUE_EXECUTION}"

          echo "Continue execution: ${CONTINUE}"

          if (env.CONTINUE == 'false') {
             echo 'Exit from jenkins pipeline'
             exit 1
          } else {
            echo 'Continue jenkins pipeline execution'
          }
        }
      }
    }

    stage('test3') {
      steps {
        script {
          if (env.BRANCH_NAME == 'master') {
            echo 'You are on master branch'
          } else {
            echo 'you are on another branch'
          }
        }
      }
    }
  }
}