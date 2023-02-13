pipeline{
  agent any
  environment{
     AWS_ACCOUNT_ID="613758335960"
	 AWS_DEFAULT_REGION="eu-west-1"
	 IMAGE_REPO_NAME="testrepo"
	 IMAGE_TAG="v${env.BUILD_NUMBER}"
	 REPOSITORY_URI="{AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
	 }
	 tools{
	   maven "maven"
	 }
	 stages {	   
	    
		    stage('Checkout Code from Git') {
               steps {
        git branch: 'main', url: 'https://github.com/ram4599/springboot-ECR.git'
                    }
                  }
		stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
              
                sh "mvn compile"
            }
        }
		stage('deploy') { 
            
            steps {
                sh "mvn clean package"
            }
        } 
	 
	    stage('Checking docker version'){
		     steps{
			    sh 'docker -v'
				}
			}
				
		stage('List the images'){
		     steps{
			    sh 'docker image ls'
				}
			}
		stage('Login to AWS ECR'){
		     steps{ 
			   // sh 'ls -lart'
			    sh 'aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 613758335960.dkr.ecr.eu-west-1.amazonaws.com'
				}
			}
		stage('Build image in ECR'){
		     steps{
			   script{
			     dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
				 }
				}
			}
		stage('Push the image'){
		     steps{
			    script{
				sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:${IMAGE_TAG}"
				sh "docker push 613758335960.dkr.ecr.eu-west-1.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
				}
				}
			}
			}
			}
