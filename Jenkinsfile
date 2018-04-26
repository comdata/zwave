pipeline {
    agent {
        docker {
            image 'maven:3.5.2-jdk-8-alpine' 
            args '-v /var/jenkins_home/.m2:/root/.m2 -v /root/.ssh:/root/.ssh' 
        }
    }
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
				sh 'mvn -T 1C -DskipTests -B clean install'
            }

		}
	
	    stage('Deploy') {
	       parallel {
	      		 stage('JUnit') {
					steps {
						junit '**/target/surefire-reports/**/*.xml'  
		            }
				}
				stage('Deploy') {
	        		steps {
	        			sh 'mvn deploy:deploy-file -Dfile=target/zwave-1.0-SNAPSHOT.jar -DpomFile=pom.xml -DrepositoryId=archiva.snapshots -Durl=http://192.168.1.36:8080/repository/snapshots'
	   				}
	   			}
	  
	   		}	
	    }
    }
}
