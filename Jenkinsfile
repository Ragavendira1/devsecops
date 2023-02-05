pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=expleoeasybuggy -Dsonar.organization=expleoeasybuggy -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=59845d7064f404cc44bca4292a5ca8aa210e49ed'
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
                 app =  docker.build("expleoecr1-dev")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://619141221545.dkr.ecr.eu-west-1.amazonaws.com', 'ecr:eu-west-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
  stage('Kubernetes Deployment of Buggy Web Application') {
	   steps {
	      withKubeConfig([credentialsId: 'kubelogin']) {
		  sh('kubectl delete all --all -n devsecops')
		  sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
		}
	      }		
  }
}
