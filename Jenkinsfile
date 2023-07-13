pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_3.8.5"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Trishabh23/remoterepo.git']])
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t trisha456/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                   sh 'docker login -u trisha456 -p ${dockerhub}'
                   } 
                   sh 'docker push trisha456/devops-integration'
                }
            }
        }
        stage('deploy to remote server'){
            steps{
                ansiblePlaybook credentialsId: 'ansibleid', installation: 'ansible', disableHostKeyChecking: true, inventory: 'inventory', playbook: 'playbook.yml'
            }
        }
    }
}
