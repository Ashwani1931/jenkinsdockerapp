pipeline {
    agent any

    stages {
        stage('Pulling The Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ashwani1931/jenkinsdockerapp'
            }
        }
        stage('Build Jar File Using Maven Tool') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Building Docker Image') {
            steps {
                sh 'sudo docker build -t ashroot/myimagetest:${BUILD_NUMBER} .'
            }
        }
        stage('Pusing the Image Into Docker HUB') {
            steps {
                withCredentials([string(credentialsId: 'docker_cred', variable: 'pass1')]) {
    // some block
                 sh 'sudo docker login -u ashroot -p  $pass1'
                 sh 'sudo docker push ashroot/myimagetest:${BUILD_NUMBER}'
}
               
            }
        }
        stage('Test Dokcer Container IN DEV ENV ') {
            steps {
                sh 'sudo docker rm -f test'
                sh 'sudo docker run -itd -p 8088:8080 --name test ashroot/myimagetest:${BUILD_NUMBER}'
            }
        }
        stage('Testing In Test Env') {
            steps {
                sshagent(['TESTENV']) {
    // some block
                sh 'sudo docker rm -f  test1'
                sh 'sudo docker run -itd -p 4545:8080 --name test1 ashroot/myimagetest:${BUILD_NUMBER}'
}
            }
        }
        stage('Verifying Our Webssite in TEST ENV') {
            steps {
                retry(7)  {
                    sh 'curl -s localhost:4545/java-web-app/ | grep -i sehwag'
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
                sh 'kubectl delete deployment  ashvini'
                sh 'kubectl create deployment ashvini --image ashroot/myimagetest:${BUILD_NUMBER}'
                sh 'wget https://raw.githubusercontent.com/ashroot/jenkinsdockerapp/main/webappsvc.yml '
                sh 'kubectl apply -f webappsvc.yml'
                }
            }
        }
    }