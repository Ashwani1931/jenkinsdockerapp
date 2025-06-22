pipeline {
    agent any

    stages {
        stage('Pulling The Code') {
            steps {
                git branch: 'Ashvini', url: 'https://github.com/Ashwani1931/jenkinsdockerapp'
            }
        }
        stage('Build Jar File Using Maven Tool') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Building Docker Image') {
            steps {
                sh 'docker build -t ashroot/myimagetest:${BUILD_NUMBER} .'
            }
        }
        stage('Pusing the Image Into Docker HUB') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred',  usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
    // some block
                 sh 'docker login -u $USERNAME -p $PASSWORD '
                 sh 'docker push ashroot/myimagetest:${BUILD_NUMBER}'
}
               
            }
        }
        stage('Test Dokcer Container IN DEV ENV ') {
            steps {
                sh 'docker rm -f test || true '
                sh 'docker run -itd -p 8089:8080 --name test ashroot/myimagetest:${BUILD_NUMBER}'
            }
        }
        stage('Verifying Our Webssite in TEST ENV') {
            steps {
                retry(7)  {
                    sh 'curl -s localhost:8089/java-web-app/ | grep -i sehwag'
                    }
            }
        }
        stage('Asking For Production Release?') {
            steps {
                input 'Release To Production? '
            }
        }
        stage('Deploy in Production Env Grade Kubernetes Cluster') {
            steps {
                sh 'kubectl delete deployment ashvini || true '
                sh 'kubectl create deployment ashvini --image ashroot/myimagetest:${BUILD_NUMBER}'
                sh 'wget https://raw.githubusercontent.com/Ashwani1931/jenkinsdockerapp/refs/heads/Ashvini/webappsvc.yml'
                sh 'kubectl apply -f webappsvc.yml'
                }
            }
        }
    } 
