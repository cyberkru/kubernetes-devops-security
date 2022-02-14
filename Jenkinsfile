pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
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