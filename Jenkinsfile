pipeline {
    agent any
    stages {
        stage('mvn'){
            steps{
                sh 'mvn clean install -Dmaven.test.skip'
            }
        }

		stage('Image') {
		    steps {
		        echo "1. Image 阶段!!!!!!!!"

		        sh 'docker build -f Dockerfile -t cloud-register .'
		    }
		}

        stage('Startup') {
            steps {
                echo "2. StartUp 阶段 !!!!232323ssss!"
                withEnv(['JENKINS_NODE_COOKIE=dontkillme']){

                  sh 'docker run -d --name={JOB_NAME} -p 8080:8080 cloud-register'
		        }
            }
        }
    }
}