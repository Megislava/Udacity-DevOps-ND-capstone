pipeline {
    agent any
    stages {
        stage('Make Kubernetes cluster in Oregon') {
			steps {
				withAWS(region:'us-west-2', credentials:'jenkins') {
					sh '''
						eksctl create cluster \
						--name capstonecluster \
						--version 1.13 \
						--nodegroup-name standard-workers \
						--node-type t2.small \
						--nodes 2 \
						--nodes-min 1 \
						--nodes-max 3 \
						--node-ami auto \
						--region us-west-2 \
						--zones us-west-2a \
						--zones us-west-2b \
						--zones us-west-2c \
					'''
				}
			}
		}
        stage('Create conf file cluster') {
			steps {
				withAWS(region:'us-west-2', credentials:'jenkins') {
					sh '''
						aws eks --region us-west-2 update-kubeconfig --name capstone-cluster
					'''
				}
			}
		}
    }
}


// withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
//                         sh 'echo "XXXXX||||||||||||XXXXX"'
//                         sh """
//                             mkdir -p ~/.aws
//                             echo "[default] >~/.aws/credentials"
//                             echo "[default] >~/boto"
//                             echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.boto
//                             echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.boto
//                             echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.aws/credentials
//                             echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.aws/credentials    
//                         """
//                         s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'udacity-aws-nd-jenkins')
//                     }