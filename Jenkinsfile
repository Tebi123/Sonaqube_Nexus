pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar-server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexuspass', groupId: 'SampleWebApp', nexusUrl: 'ec2-54-234-104-9.compute-1.amazonaws.com:9000', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
             deploy adapters: [tomcat9(credentialsId: 'tompass', path: '', url: 'http://54.164.2.106:8080/')], contextPath: 'myapp', war: '**/*.war'
              
              
              
          }
            
        }
            
        }
} 
