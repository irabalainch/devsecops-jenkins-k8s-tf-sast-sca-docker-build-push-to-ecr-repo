pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=aebuggytest -Dsonar.organization=aebuggytest -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=4542854978d0e5cafd381ae57939d7c32d426799'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://080726094508.dkr.ecr.eu-west-1.amazonaws.com/', 'ecr:eu-west-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
