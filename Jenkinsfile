pipeline {
    agent any
    stages {


		stage('Image') {
		    steps {
		        echo "1. Image 阶段 !!"
		        pom = readMavenPom file: 'pom.xml'
		        img-name = "${pom.groupId}-${pom.artifactId}"
		        echo "img-name : ${img-name}"
		        sh 'docker build -f Dockerfile -t cloud-register:1.0.0'
		    }
		}
        stage('Startup') {
            steps {
                echo "2. StartUp 阶段 !!!!!"
                withEnv(['JENKINS_NODE_COOKIE=dontkillme']){

                  sh 'docker run -d --name=cloud-register-demo -p 8080:8080 cloud-register:1.0.0'
		        }
            }
        }
    }
}