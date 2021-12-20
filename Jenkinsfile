pipeline {
    agent any

    stages {
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
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh 'mvn package'
                echo '2nd build'
            }
        }
    

        
  /*
        stage('Copy war') {
            steps {
                echo 'copying ...'
                sh 'sudo cp /var/lib/jenkins/workspace/project/target/*.war /home/centos/'
                echo 'copied'
            }
       }

    }  
  */
        
  
     stage('Build and push Docker images..') {
      steps{
       sh "sudo docker image build -t $JOB_NAME:v1.$BUILD_ID /var/lib/jenkins/workspace/project/."
       sh "sudo docker image tag $JOB_NAME:v1.$BUILD_ID padharthiswetha/$JOB_NAME:v1.$BUILD_ID"
       sh "sudo docker image tag $JOB_NAME:v1.$BUILD_ID padharthiswetha/$JOB_NAME:latest"
       sh "sudo docker image push padharthiswetha/$JOB_NAME:v1.$BUILD_ID"
       sh "sudo docker image push padharthiswetha/$JOB_NAME:latest"
       sh "sudo docker image rmi $JOB_NAME:v1.$BUILD_ID padharthiswetha/$JOB_NAME:v1.$BUILD_ID padharthiswetha/$JOB_NAME:latest"
      }
  
  }
 }
}
/*
        
        stage('Run ansible'){
            steps{
                sh 'ansible-playbook depl.yml'
            }
        }
        
    }
}
*/

