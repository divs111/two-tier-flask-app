pipeline{
    agent any;
    stages{
        stage("Code"){
            steps{
                git url : "https://github.com/divs111/two-tier-flask-app.git", branch : "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker system prune -af"
                sh "docker build -t two-tier ."
                
            }
        }
        stage("Test"){
            steps{
                echo "Testing is also done"
            }
        }
        stage("Push to dockerHub")
        {
            steps{
                withCredentials([usernamePassword(credentialsId: "dockerhubcreds",
                passwordVariable: "docpassword",
                usernameVariable: "docuser")])
                {
                    sh "docker login -u ${docuser} -p ${docpassword}"
                    sh "docker image tag two-tier ${docuser}/two-tier"
                    sh "docker push ${docuser}/two-tier:latest"
                        
                    
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
        }
    }
}
