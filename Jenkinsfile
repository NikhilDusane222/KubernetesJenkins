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
				#docker-compose down 
				docker stop $(docker ps -a -q)
				docker rm $(docker ps -a -q)
				docker rmi -f 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp
				docker-compose up -d
				#docker build -t chat .
				'
				'''
			}
		}
 
	 stage('Push Image') {
			steps { 
				sh ''' #! /bin/bash
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@52.66.240.187 '
				docker tag chatapp13_chat 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chatapp13_chat
				docker push 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chatapp13_chat
				#docker tag chat:latest 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chat
				#docker push 760496128264.dkr.ecr.ap-south-1.amazonaws.com/chatapp:chat
				'
				'''
			}
		}	
		
         stage('Deploy') { 
               steps {
                 sh ''' #! /bin/bash 
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
