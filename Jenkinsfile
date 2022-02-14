pipeline {
  agent any
  environment {
  	sonar_token = credentials('sonar_token')
  }

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        } 

       stage('Sonarqube Scan') {
       		steps {
       			script{
       				def scannerhome = tool 'SonarQube';
       				withSonarQubeEnv('SonarQubeServer'){
       					sh "${tool("SonarQube")}/bin/sonar-scanner \
       					-Dsonar.projectKey=kubernetes-devops-numeric \
       					-Dsonar.sources=. \
       					-Dsonar.css.node=. \
       					-Dsonar.host.url=http://192.168.1.17:9000/ \
       					-Dsonar.login=$sonar_token"
       				}
       			}
       		}
       }

       stage('Docker Build') {
	      steps {
	        // withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
	          sh 'printenv'
	          //sh 'docker build -t cyberkru/numeric-app:""$GIT_COMMIT"" .'
	          sh 'docker build -t cyberkru/numeric-app:latest .'
	          // sh 'docker push cyberkru/numeric-app:""$GIT_COMMIT""'
	        // }
	      }
	    }  
    }
}