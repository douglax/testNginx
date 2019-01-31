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
	}
	post{
		always{
			// deleteDir()
			echo 'Post steps'
		}
	}
}
