pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven_3_0_5"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
               //git 'https://gitlab.com/krashnat922/webAppExample.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                //build 'webAppExamplePS'
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
        
        
    
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat7(credentialsId: '46d3939a-e669-4329-817b-23c5e98741a5', path: '', url: 'http://192.168.254.138:9090')], contextPath: 'webAppExamplePS', war: '**/*.war'
            }
        }
    }
}
