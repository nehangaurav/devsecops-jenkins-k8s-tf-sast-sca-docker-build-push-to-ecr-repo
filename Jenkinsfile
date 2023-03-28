pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp23 -Dsonar.organization=asgbuggywebapp23 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=b14f80f41b614ef04bc33cb87367d01951de006b'
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
               withDockerRegistry([credentialsId: "nehangaurav", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://862283187998.dkr.ecr.ap-south-1.amazonaws.com/', 'ecr:ap-south-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
