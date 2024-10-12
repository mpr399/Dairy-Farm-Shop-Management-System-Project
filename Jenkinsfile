pipeline {
    agent any
    stages {
        stage('checkout'){
            steps{
                script{
                    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/mpr399/https://github.com/mpr399/Dairy-Farm-Shop-Management-System-Project.git'
                }
            }
        }
        stage('build'){
            steps{
                script{
                    sh 'cd $HOME/workspace/dfsms/app && docker build -t mvpar/dfsmsapp .'
                    sh 'cd $HOME/workspace/dfsms/db && docker build -t mvpar/dfsmsdb .'
                }
            }
        }
        stage('push'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'REGISTRY_PWD', usernameVariable: 'REGISTRY_USER')]) {
                        sh 'docker login -u=$REGISTRY_USER -p=$REGISTRY_PWD'    
                        sh 'docker push mvpar/dfsmsapp'
                        sh 'docker push mvpar/dfsmsdb'
                    }    
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                     sh "cd $HOME/workspace/dfsms && docker-compose down && docker-compose up -d"
                }
            }
        }
    }
}