pipeline{
    agent any;
    
    stages{
        stage("Code Clone/Pull"){
            steps{
                git url: "https://github.com/SachinDubey0307/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Code Build by Docker"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "test cases given by QA/DEV TEAM"
            }
        }
        stage("Push To Docker-Hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                passwordVariable: "dockerHubPass",
                usernameVariable: "dockerHubUser"
                )]){
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass} "
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                
                }
            }
        }
        stage("Deployment"){
            steps{
            sh "docker compose up -d --build flask-app"
            }
        }
    }
}
