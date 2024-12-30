pipeline {
    agent {
	node {
        label  'node1'
        }
		}

    tools {
        // Install the Maven version configured as "M3" and add it to the path
        maven "Maven 3.6.3"
    }

    stages {
        stage('Code Clone') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ksrepo9/sp24.git'
            }
			}
		stage('mvn version') {
            steps {
              sh 'mvn --version'    
            }
			}	
		stage('mvn clean') {
            steps {
                sh 'mvn clean'      
            }
			}
		stage('mvn validate') {
            steps {
                sh 'mvn validate'      
            }
			}
        stage('SonarScan') {
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=kspro -Dsonar.host.url=http://192.168.29.30:9000 -Dsonar.login=sqp_727cd2c07493015efb63de36da645a0faaf5ae01'      
            }
			}

			
		stage('Monitor Project') {
            steps {                    
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
          
                    sh """
					    chmod +x ./mvnw                       
					    snyk auth 492bdf6f-8301-4d41-b8bf-6d4fa4fb8ddb
						snyk code test
						snyk test --json --severity-threshold=low
                        snyk monitor --org=ksrepo9 --project-id=492bdf6f-8301-4d41-b8bf-6d4fa4fb8ddb --json > report.json
                    """
                    echo "Snyk monitoring completed successfully."
                }          
                
              

            }
            }
		        

		stage('mvn Compile') {
            steps {
                sh 'mvn compile'      
            }
			}
		stage('mvn Test') {
            steps {
                sh 'mvn test'      
            }
			}
        			
		stage('mvn Package') {
            steps {
                sh 'mvn package'      
            }
			}
		stage('mvn Deploy') {
            steps {
                sh 'mvn deploy'      
            }
			}		         
        }
     post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}


