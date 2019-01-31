pipeline{
	agent any 
	environment{
		//ORG_GRADLE_PROJECT_octoApiKey = credentials('octopus_apikey')
		appNuGetVersionV2 = ' '
	}
	stages{
		stage('Build image'){
			steps{
				sh 'docker build -t sotolito/testnginx .'
			}
			post{
				success{
					echo 'Image created successfully'
				}
			}
		}
	    stage('Create container'){
			steps{
				sh 'docker run -i -t -d --name=webtest sotolito/testnginx /bin/bash'
			}
			post{
				success{
					echo 'Image created successfully'
				}
			}
		}
	    stage('Test webserver'){
			steps{
				script{
                    env.CONTAINERIP = docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
                }
                sh 'curl "$(CONTAINERIP)"'
                echo "$(CONTAINERIP)"
			}
			post{
				success{
					echo 'Web Server tested successfully'
				}
			}
		}
	    stage('Destroy infra'){
			steps{
				sh 'docker stop webtest'
                sh 'docker rm webtest'
                sh 'docker rmi sotolito/testnginx'
			}
			post{
				success{
					echo 'Infrastructure destroyed'
				}
			}
		}
/*		stage('Connectivity Check'){
			steps{
				sh 'ping -i 1 -t 1 -c 3 moximo.rocks'
			}
			post{
				success{
					echo 'Ping Successful'
				}
			}
		}
		stage('Website Check'){
			steps{
				sh 'ab -t 1 -s 1  -c 10 -n 10 http://moximo.rock/'
			}
			post{
				success{
					echo 'Website check successful'
				}
			}
		}
		stage('Deploying infrastructure'){
			steps{
				sh 'whoareyoubitch'
			}
			post{
				success{
					echo 'infra deployed Successful'
				}
			}
		}		
		stage('Deploying App'){
			steps{
				sh 'id'
			}
			post{
				success{
					echo 'App deployed successfully'
				}
			}
		}

*/
	}
	post{
		always{
			// deleteDir()
			echo 'Post steps'
		}
	}
}
