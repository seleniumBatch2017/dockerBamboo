#!/usr/bin/env groovy
  
  pipeline {
 
   agent any 
   tools {
 		maven 'M3'
 		jdk 'JDK1.8'
		} 
	stages{
        stage('Git Check-Out') {
            steps {
                   git 'https://github.com/seleniumBatch2017/dockerBamboo.git'
            }
        }
        stage('Spin-Up Selenium-Grid Containers') {
            steps {
                   sshagent(['c4725c71-bcfb-49e0-b8fa-58e9e1529dea']) {
    				sh 'ssh -tt jenkins@35.190.151.173  /usr/local/bin/start-grid.sh'
   				   }
            }
        }
        stage('Run Automated Tests') {
            steps {
         			sh 'mvn -Dmaven.test.failure.ignore clean verify'
            }
        }
        stage('Generate Serenity Report') {
            steps {
    			 publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/serenity', reportFiles: 'index.html', reportName: 'Serenity Report', reportTitles: ''])
            }
        }
   }
    
    post {
        always {
					sshagent(['c4725c71-bcfb-49e0-b8fa-58e9e1529dea']) {
    				sh 'ssh -tt jenkins@35.190.151.173 /usr/local/bin/stop-grid.sh'
   				   } 
   		       }
        failure {
        	sh 'echo hello'
           // mail to: team@example.com, subject: 'The Pipeline failed :('
        		}
    }
    
}

 
   
   
