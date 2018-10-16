pipeline {
    agent {
        docker {
            image 'maven:3.5.2-jdk-8-alpine' 
            args '-v /var/jenkins_home/.m2:/root/.m2 -v /root/.ssh:/root/.ssh' 
        }
    }
	 triggers { upstream(upstreamProjects: 'obera-base', threshold: hudson.model.Result.SUCCESS) }
    stages {
		stage('Prepare') {
		    steps {
			sh 'apk update'
			sh 'apk add rsync openssh openrc git'
			
		    } 
		}
	
		stage('Build dependencies') {
		    steps {
			sh 'rm -rf /root/git'
			sh 'mkdir -p /root/git'
		    }
		}

		stage('Build') { 
			steps {
				withMaven() {
					sh 'mvn -T 1C -DskipTests -B clean deploy'
				}
            }

		}
	

	  
	
    }
}
