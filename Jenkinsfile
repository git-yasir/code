pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/git-yasir/code.git", branch: "main"
                echo "**************************************************************************************************************"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t yas-nginx-app ."
                echo "**************************************************************************************************************"
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag yas-nginx-app ${env.dockerHubUser}/yas-nginx-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/yas-nginx-app:latest"
                echo "**************************************************************************************************************"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                echo "**************************************************************************************************************"
                
            }
        }
    }
}
