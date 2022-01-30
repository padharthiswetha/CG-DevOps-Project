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
                sh 'cd /var/lib/jenkins/workspace/project/'
                sh 'mvn deploy   nexus:nexus'
            }
        }
     }
}


