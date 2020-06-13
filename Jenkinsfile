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
				ssh -i /var/lib/jenkins/.ssh/id_rsa root@13.233.230.62 '
				docker tag dockerpipeline_chatapplication 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp
				docker push 296838539158.dkr.ecr.ap-south-1.amazonaws.com/chatapplication:chatapp
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
