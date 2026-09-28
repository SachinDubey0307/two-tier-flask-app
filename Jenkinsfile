pipeline{
    agent any;
    stages{
        stage("pull/clone form Github"){
            steps{
               git url: "https://github.com/SachinDubey0307/two-tier-flask-app.git",branch: "master"
            }
        }
        stage("teting"){
           steps{
               echo "Unit testing done"
            }
        }
        stage("build from dockerfile"){
               steps{
             sh "docker build -t flaskapp:latest ."
             echo "you have to add current logged in user and jenkins user to docker group due to docker permission issue"
             echo "sudo usermod -aG docker ubuntu"
             echo "sudo usermod -aG docker jenkins"
             echo "sudo systemctl restart jenkins"
            }
        }
        stage("push Image to Docker Hub"){
    steps{
        withCredentials([usernamePassword(
            credentialsId: "dockerhubcreds",
            usernameVariable: "dockerHubUser",
            passwordVariable: "dockerHubPass"
        )]){
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
            sh "docker image tag flaskapp:latest ${env.dockerHubUser}/flaskapp:latest"
            sh "docker push ${env.dockerHubUser}/flaskapp:latest"
        }
    }
}
        stage("Deploye on EKS Cluster/Docker Compose"){
            steps{
            echo "sudo apt-get install docker-compose-v2"
            echo "docker compose version"
            sh "docker compose up -d --build flask-app"
            }
        }
    }
}
