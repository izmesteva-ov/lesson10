pipeline {
    agent {
       docker {
         image '51.250.59.7:8082/jenkins-agent'
         args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
    }
  } 	
  stages {    
    stage('git clone') {
        steps {
             git 'https://github.com/izmesteva-ov/original11.git'
         } 
      }
    stage('Build war') {
        steps {
           sh 'mvn package'
      }
    }
    stage('Docker image') {
         environment {
             REPO_URL = '51.250.59.7:8082'                              
        }	  	    
        steps {
	     withCredentials([usernamePassword(credentialsId: 'e960d181-5wcb-35fc-cc09-096e0f5q1201', passwordVariable: 'passwd', usernameVariable: 'user')]) {              	
          sh 'docker build -t myapp .'
          sh 'docker login -u ${user} -p ${passwd} $REPO_URL'
          sh 'docker tag myapp $REPO_URL/myapp'
          sh 'docker push $REPO_URL/myapp'
       }
    }
  }	    
    stage('Run docker on dem02') {
       environment {
            REPO_URL = '51.250.59.7:8082'                              
        }	  
       steps {
	     withCredentials([usernamePassword(credentialsId: 'e960d181-5wcb-35fc-cc09-096e0f5q1201', passwordVariable: 'passwd', usernameVariable: 'user')]) {    
         sh 'ssh-keyscan -H 51.250.59.105 >> ~/.ssh/known_hosts'
         sh '''ssh root@51.250.59.105 << EOF
	sudo docker login -u ${user} -p ${passwd} $REPO_URL
	sudo docker pull $REPO_URL/myapp
	sudo docker run -d -p 8080:8080 $REPO_URL/myapp
EOF'''
      }
    }	
 }
  
  }
  
  }
