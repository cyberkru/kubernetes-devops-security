pipeline {
  agent any
  environment {
  	sonar_token = credentials('SONAR_TOKEN')
  }

  stages {
      // stage('Build Artifact') {
      //       steps {
      //         sh "mvn clean package -DskipTests=true"
      //         archive 'target/*.jar' 
      //       }
      //   } 

       stage('Sonarqube SAST') {
       		steps {
       			withSonarQubeEnv('SonarQube') {
       				sh "mvn clean verify sonar:sonar \
	  				-Dsonar.projectKey=kubernetes-devops-numeric \
	  				-Dsonar.host.url=http://192.168.1.23:9000 \
	  				-Dsonar.login=$sonar_token"
       			}
       			timeout(time: 2, unit: 'MINUTES') {
		          script {
		            waitForQualityGate abortPipeline: true
		          }
		        }
       			
       		}
       }

       stage('Vulnerability Scan - Docker ') {
	      steps {
	        sh "mvn dependency-check:check"
	      }
	      post {
	        always {
	          dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
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