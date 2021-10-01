def name;
pipeline {
    agent any
    stages {
        stage('Build Stage') {
            steps {
                script{
                bat "mvn -B -DskipTests clean package"
                  }
            }
        }
                stage('Testing Stage') {
		        steps {
		          	script {
				        bat "mvn test"
                  }
            }
        }
       }
     }
        
