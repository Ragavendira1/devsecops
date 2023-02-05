pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=Ragavendira1_devsecops -Dsonar.organization=Ragavendira1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=59845d7064f404cc44bca4292a5ca8aa210e49ed'
			}
        } 
  }
}
