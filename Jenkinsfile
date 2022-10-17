pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "3.8.6"
    }

    stages {   
        stage('checkout') {
            steps {
                git credentialsId: '9d4ef510-e81b-4398-b80d-920b8cf514fa', url: 'https://github.com/Shivshive/testng-failed.git'
            }
        }
        
        stage('check maven') {
            
            steps {
                bat 'mvn -version'
            }
        }
        
        stage('clean and test') {
            
            steps {
                
                script {
                    
					catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
						bat 'mvn clean test -Dsuite=testng.xml -Dreportoutput_dir=${env.BUILD_NUMBER}_s'
					}
					
                }
            }
        }
        
        stage('rerun failed case') {
            
            when {
                expression {
                    fileExists('target/surefire-reports/testng-failed.xml')
                }
            }
            
            steps{
                bat 'mvn test -Dsuite=target/surefire-reports/testng-failed.xml -Dreportoutput_dir=${env.BUILD_NUMBER}_rr'
            }
        }
    }

    
}
