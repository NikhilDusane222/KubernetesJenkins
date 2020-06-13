pipeline {
	agent any
	stages {
		stage('Clone Repository') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@13.233.230.62 '
				rm -rf /dockerpipeline/
				'
				scp -r /var/lib/jenkins/workspace/dockerpipeline root@13.233.230.62:
				'''
			}
		}
		stage('Build Image') {
			steps {
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@13.233.230.62 '
				cd dockerpipeline/
				 $(aws ecr get-login --registry-ids 296838539158 --no-include-email --region  ap-south-1)
				docker stop chatapplication
				docker rm chatapplication
				docker rmi -f 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp chatapp:latest
				docker build -t chatapp .
				'
				'''
			}
		}
 
	 stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@13.233.230.62 '
				docker tag chatapp 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp
				docker push 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp
				'
				'''
			}
		}	
		
         stage('Deploy') { 
               steps {
                    sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa ubuntu@13.233.162.19 '
				kubectl delete $(kubectl get po -o=name | grep chat-deployment)
				'
				echo Deploy Successfull
				'''
                }
            }

         stage('status'){
                steps {
                sh ''' #! /bin/bash
                echo Deployment started
                '''
                }  
            }

        }
        post { 
            always { 
                echo 'Stage is success'
            }
        }    
    }
