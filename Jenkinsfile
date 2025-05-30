pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-app"
        RELEASE = "1.0.0"
        DOCKER_USER = "raghukunchum"
        DOCKER_PASS = 'Docker_2go$$'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
	
    }
	//  cleaning the workspace.
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
	    // Tell git to checkout the code
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/clkrgs/a-reddit-clone.git'
            }
        }
	    // Enter sonarqube analysis details
	    /*-Dsonar.projectName=Reddit-Clone-CI \
                    -Dsonar.projectKey=Reddit-Clone-CI'''*/
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('SonarQube-Token') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
		
		  
      		
                }
            }
        }
	    //scanning even it fails want to run so giving false
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
                }
            }
        }
	    //Need dependencies NPM
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
	    //Need Trivy scan with trivy command 
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
             }
         }
    }
}
/*
	stage("Build & Push Docker Image") {
	  steps{
            script{
		docker.withRegistry('',DOCKER_PASS) {
		  docker_image = docker.build "${IMAGE_NAME}"
		}
		  docker.withRegistry('',DOCKER_PASS) {
		  docker_image.push("${IMAGE_TAG}")
		  docker_image.push('latest')
	    }
	}
     }
}
    }
}

  stage("Trivy Image Scan") {
		steps {
			script {
				sh('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image rbmihawk/reddit-clone-app:latest --no-progress --scanners vuln --exit-code 0 --severity HIGH,CRITICAL --format table > trivyimage.txt')
			}
		}
	 }
    }
}
   stage('Cleanup Artifacts') {
		    steps {
			    script{
				    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
				    sh "docker rmi ${IMAGE_NAME}:latest"
			  }
		    }
	  }
	    stage("Trigger CD Pipeline") {
            steps {
                script {
                    sh "curl -v -k --user jenkins:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-54-174-230-113.compute-1.amazonaws.com:8080/job/Reddit-Clone-CD/buildWithParameters?token=gitops-token'"
                }
            }
         }
     }    

post {
    always {
        emailext(
            attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                  "Build Number: ${env.BUILD_NUMBER}<br/>" +
                  "URL: ${env.BUILD_URL}<br/>",
            to: 'rbandela1704@gmail.com',
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
	       
                )
         }
    }
}
*/
