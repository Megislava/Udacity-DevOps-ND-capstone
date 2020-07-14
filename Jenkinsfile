pipeline {
    agent any

    stages {
        stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
              }
        }

        stage('docker') {
            steps {
                sh "docker build --tag=index ."
            }
        }

        stage('push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_id', passwordVariable: 'DOCKER_REGISTRY_PWD', usernameVariable: 'DOCKER_REGISTRY_USER')]) {
                  sh 'docker login -u=$DOCKER_REGISTRY_USER -p=$DOCKER_REGISTRY_PWD'
                  sh 'docker tag index megislava/index:latest'
                  sh "docker push megislava/index"
                }
            } 
        }

        stage('deploy') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                   sh './cloudformation/create.sh capstone-projekt cloudformation/infrastructure.yml cloudformation/params.json'
                }
            }
        }
    }
}