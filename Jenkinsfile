pipeline {
    agent any

    tools {
        maven "MAVEN_HOME"
    }

    stages {
        stage('Build') {
            steps {
               
                 sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        

            post {
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
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://192.168.0.132:8090')], contextPath: '/rps', war: '**/*.war'
            }
            post {
                success{
                    emailext body: 'Deployment successful', compressLog: true, recipientProviders: [developers()], subject: 'Build result', to: 'sanket.padgal@afourtech.com'
                }
            }
        }
    }
}
