pipeline {
	agent any
	
	environment {

	  	GITHUB_ORGANIZATION = "p9sys"	
	  	GITHUB_REPO = "webapp01"
	  	DOCKERHUB_REGISTRY = "pf9sys/webapp01"
 	}
    
    stages {
      
    	stage ('PREPARATION') {
        	steps {
      
          		cleanWs()
          		git url: "https://github.com/${GITHUB_ORGANIZATION}/${GITHUB_REPO}"
	        }
	    }


      	stage ('BUILD') {
        	steps {

          		sh 'npm config rm proxy'
                        sh 'npm config rm https-proxy'
              
        	}
        }	

        stage ('PUBLISH') {
        	environment {
           
            	registryCredential = 'dockerhub'
          	}
          	steps {

               	 	script {
				def appimage = ("${DOCKERHUB_REGISTRY}:${env.BUILD_ID}")
				docker.build("${DOCKERHUB_REGISTRY}:${env.BUILD_ID} --network=host")	
                    		docker.withRegistry( '', registryCredential ) {
                        	appimage.push()
                        	appimage.push('latest')
                    	}
              	  	}

          	}

        }

        stage ('DEPLOY') {
        	steps {

        		sh 'kubectl apply -f k8s/app.yaml'
        	}
        }
    }
}
