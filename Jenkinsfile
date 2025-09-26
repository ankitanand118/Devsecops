
pipeline {
  agent { label 'build' }
   environment { 
        registry = "ankitanand118/democicd" 
        registryCredential = 'dockerhub' 
   } 
	tools{
        maven 'maven3'
		
        }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/ankitanand118/Devsecops.git'
      }
    }
  
   stage('Stage I: Build') {
      steps {
        echo "Building Jar Component ..."
        sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64; mvn clean package "
      }
    }

   stage('Stage II: Code Coverage ') {
      steps {
	    echo "Running Code Coverage ..."
        sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64; mvn jacoco:report"
      }
    }
   
   stage('Stage VI: Build Image') {
      steps { 
        echo "Build Docker Image"
        script {
               docker.withRegistry( '', registryCredential ) { 
                 myImage = docker.build registry
                 myImage.push()
                }
        }
      }
    }
        
   stage('Stage VII: Scan Image ') {
      steps { 
        echo "Scanning Image for Vulnerabilities"
        sh "trivy image --scanners vuln --offline-scan ankitanand118/democicd:latest > trivyresults.txt"
        }
    }
          
   stage('Stage VIII: Smoke Test ') {
      steps { 
        echo "Smoke Test the Image"
        sh "docker run -d --name smokerun -p 8080:8080 adamtravis/democicd"
        sh "sleep 90; ./check.sh"
        sh "docker rm --force smokerun"
        }
    }

  }

}







