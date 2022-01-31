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
                echo ' Pushing artifact to Nexus repo'
               nexusArtifactUploader artifacts: [
                   [
                       artifactId: 'WebAppCal',
                       classifier: '',
                       file: 'target/WebAppCal-1.0.war',
                       type: 'war'
                   ]
               ], 
                   credentialsId: 'nexus', 
                   groupId: 'com.web.cal',
                   nexusUrl: '18.217.166.233:8081',
                   nexusVersion: 'nexus3',
                   protocol: 'http', 
                   repository: 'maven-nexus',
                   version: '1.0'
                
            }
        }
     }
}


