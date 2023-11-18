pipeline {
  agent any
  tools { 
        maven 'maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=kloud45 -Dsonar.organization=kloud45 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=8f444a51a34c57923785dfdbd7ef2aba4eb722b4'
			}
        }
	   stages('RunSCAAnalysisUsingSynk') {
		   steps {
			   withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
				   sh 'mvn snyk:test -fn'
				   
			   }
		   }
	   }
  }
}
