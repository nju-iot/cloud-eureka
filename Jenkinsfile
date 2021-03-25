pipeline {
    agent any
    environment{
        imageName:cloud-register
        jobName:cloud-register
    }
    stages {
        stage('mvn'){
            steps{
                sh 'mvn clean install -Dmaven.test.skip'
            }
        }

		stage('Image') {
		    steps {
		        echo "1. Image 阶段!!!!!!!!"
                sh'''
                 #获取同名容器ID
                containerStr = `docker ps -a | grep -w  ${imageName} | awk '{print $1}'`

                #获取同名镜像ID
                imageStr = `docker images | grep -w ${imageName} | awk '{print $3}'`

                echo "container id:$containerStr"
                echo "image id:$imageStr"

                if [ "$imageStr" !=  "" ] ; then
                    if [ "$containerStr" !=  "" ] ; then
                    	#停掉容器
                    	docker stop `docker ps -a | grep -w ${imageName}  | awk '{print $1}'`

                    	#删除容器
                    	docker rm `docker ps -a | grep -w ${imageName}  | awk '{print $1}'`

                    	#删除镜像
                    	docker rmi --force ${imageName}
                    else
                    	#删除镜像
                    	docker rmi --force ${imageName}

                    fi
                fi

                '''
		        sh 'docker build -f Dockerfile -t cloud-register .'
		    }
		}

        stage('Startup') {
            steps {
                echo "2. StartUp 阶段 !!!!232323ssss!"
                withEnv(['JENKINS_NODE_COOKIE=dontkillme']){

                  sh 'docker run -d --name={jobName} -p 8080:8080 cloud-register'
		        }
            }
        }
    }
}