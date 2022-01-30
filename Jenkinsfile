pipeline {
    agent any
    
    stages {
       stage('Setup parameters') {
           steps {
               script {properties([parameters([choice(choices: 'master\nfeature', description: 'select the branch to build ', name: 'branch')])])}
           }
       }
       stage('Validate') {
            steps {
                echo 'Validate Code'
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'mvn test'
            }
        }
        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar'){
                sh 'mvn clean package sonar:sonar'
                }
                echo 'Sonar Analysis done: Results at Sonar Server'
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh 'mvn package'
            }
        }
          stage('Deploy to Nexus') {
            steps {
               nexusArtifactUploader artifacts: [
                   [
                       artifactId: 'WebAppCal', 
                       classifier: '', 
                       file: 'target/WebAppCal Maven Webapp-1.0.war', 
                       type: 'war'
                    ]
               ], 
               credentialsId: 'nexus3', 
               groupId: 'com.web.cal', 
               nexusUrl: 'http://3.17.161.88:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: 'maven-nexus', 
               version: '1.0'
            }
        }
     }
}


