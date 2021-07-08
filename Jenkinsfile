pipeline {
    agent any

    tools {
        maven "Maven_3_0_5"
    }

    stages {
        stage('Build') {
            steps {
               //git 'https://gitlab.com/krashnat922/webAppExample.git'

                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                //build 'webAppExamplePS'
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
                deploy adapters: [tomcat7(credentialsId: '46d3939a-e669-4329-817b-23c5e98741a5', path: '', url: 'http://192.168.254.138:9090')], contextPath: 'webAppExamplePS', war: '**/*.war'
            }
            post {
                success{
                    emailext body: 'Deployment is successul', recipientProviders: [developers()], subject: 'webAppExample', to: 'krishna.tapdiya@afourtech.com'
                }
            }
        }
    }
}
