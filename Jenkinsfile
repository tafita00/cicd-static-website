  pipeline{
    environment{
        IMAGE_NAME = "staticwebsite"
        APP_CONTAINER_PORT = "5000"
        APP_EXPOSED_PORT = "80"
        IMAGE_TAG = "latest"
        DOCKERHUB_ID = "tafitasoa0"
        DOCKERHUB_PASSWORD = credentials('dockerhub_password')
    }
    agent none
    stages{
        stage('Build Image'){
            agent any
            steps {
                script {
                    sh 'docker build -t ${DOCKERHUB_ID}/$IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }
        stage('Run container basedon builded image'){
            agent any
            steps{
                script{
                    sh '''
                        echo "Cleaning existing container if exist"
                        docker ps -a | grep -i $IMAGE_NAME && docker rm -f $IMAGE_NAME
                        docker run --name $IMAGE_NAME -d -p $APP_EXPOSER_PORT:$APP_ECONTAINER_PORT -e PORT=$APP_CONTAINER_PORT ${DOCKERHUB_ID}/$IMAGE_NAME:$IMAGE_TAG
                        sleep 5
                    '''
                }
            }
        }
        stage('Test image'){
            agent any
            steps {
                script {
                    sh '''
                        curl 172.17.0.1 | grep -i "Dimension"
                    '''
                }
            }
        }   
        stage('Clean container'){
            agent any
            steps{
                script{
                    sh '''
                        docker stop $IMAGE_NAME
                        docker rm $IMAGE_NAME
                    '''
                }
            }
        }
        stage('Login and Push Image on docker hub'){
            agent any
            steps{
                script{
                    sh '''
                        echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_ID --password-stdin
                        docker push ${DOCKERHUB_ID}/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
    }
 }
