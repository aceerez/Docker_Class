pipeline {
  agent { node { label 'slave01' } }

   environment {	
		IMAGE_NAME="${Language}"
		REPO_NAME="${DOCKER_HUB_NAME}/${IMAGE_NAME}"   
		USER=credentials('DOCKERHUB_USER')
		PASS=credentials('DOCKERHUB_PASSWORD')	
	}   

    stages {
      stage('Clone Sources') {
        steps {
          checkout scm
        } 
      }
	  stage('Checking environment') {
         steps {
            sh 'printenv'
         }
      }
		 stage('Selected language') {
			 steps {
				echo "You choose to code in ${Language}"
			 }
		  }
		  stage('Build a docker image for all') {
		   when { expression {return (params.Language == 'All') }
		   }
         steps {
            echo 'Build process for all ..'            
            sh '''
                cd docker
                docker build -t="${IMAGE_NAME}:${BUILD_NUMBER}" -f Dockerfile_all .
            '''
         }
      }
	  
	  stage('Build a docker image for Python') {
       when { expression {return (params.Language == 'Python') }
	   }
	   steps {
		    echo 'Build process for Python ..'            
            sh '''
                cd docker
                docker build -t="${IMAGE_NAME}:${BUILD_NUMBER}" -f Dockerfile_Python .
            '''
	   }
		 
         }
      
      stage('Build a docker image for C') {
	      when { expression {return (params.Language == 'C') }
	   }
         steps {
           echo 'Build process for C ..'            
            sh '''
                cd docker
                docker build -t="${IMAGE_NAME}:${BUILD_NUMBER}" -f Dockerfile_C .
            '''
         }
      }
      stage('Build a docker image for Bash') {
	      when { expression {return (params.Language == 'Bash') }
	   }
         steps {
            echo 'Build process for C ..'            
            sh '''
                cd docker
                docker build -t="${IMAGE_NAME}:${BUILD_NUMBER}" -f Dockerfile_Bash .
            '''
         }
      }
	  stage('Push a docker image') {
         steps {
			sh '''
				echo "Login to Docker Hub"
				docker login -u ${USER} -p ${PASS} 
				docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${REPO_NAME}:${BUILD_NUMBER}
				docker push ${REPO_NAME}:${BUILD_NUMBER}
				echo "Pushing the latest version of the image"
				docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${REPO_NAME}:latest
				docker push ${REPO_NAME}:latest
			'''
         }
      }
	  
	  
	      stage('Saving Log') {
         steps {
		 echo 'Saving LOG Results ..'
		 sh '''
		      log_file="${HOME}/Documents/logs/logFile.txt"
		      mkdir -p ${HOME}/Documents/logs/              
		      if [ -f "${log_file}" ]; then
			echo "file ${log_file} exists"
		      else
			      touch ${log_file}
		      fi  
		      echo "Build start at $(date) " >> ${log_file}
		      echo "Build Number $BUILD_NUMBER" >> ${log_file}		      
		      echo "#############################" >> ${log_file}
            '''
         }
      }
      
          
   
  }
  }
  
