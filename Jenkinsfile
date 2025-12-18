
pipeline {
	agent any
	tools {
	    maven "MAVEN3.9"
	    jdk "JDK17"
	}

	stages {
	    stage('Fetch code') {
            steps {
               git branch: 'cicd-selenium', url: 'https://github.com/jkabirqa/ECommerceProject.git'
            }

	    }


	    stage('Build'){
	        steps{
	           sh 'mvn install -DskipTests'
	        }

	     
	    }

	    stage('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
        }
        


          /* ==== UI TESTING ==== */ 
       stage('UI-Test -Selenium'){
          steps{
            sh 'mvn -pl qa-tests/ui-tests test'
          }
          post{
            always{
               junit '**/ui-tests/target/surefire-reports/*.xml'
               archiveArtifacts 'target/*.jar'
            }
          }
       }



	}

  post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#devopscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }

}
