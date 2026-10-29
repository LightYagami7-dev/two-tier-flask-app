
pipeline{
    
    agent {label "dev"}
    stages{
        stage("Code"){
            steps{
               git url:"https://github.com/LightYagami7-dev/two-tier-flask-app.git" , branch:"master"
            }
        }
        stage("Trivy filesysytem scan"){
             steps{
                 sh "trivy fs . -o result.json "
             }
            
        }
         stage("Build"){
             steps{
                 sh "docker build -t two-tier-flask-app ."
             }
            
        }
         stage("Push to docker hub"){
             steps{
              withCredentials([usernamePassword(
                  credentialsId :"dockerHubCreds",
                  passwordVariable:"dockerHubPass",
                  usernameVariable:"dockerHubUser"
                  )]){
                      sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                      sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                      sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                  }
             }
        }
         stage("Deploy"){
             steps{
                 sh "docker compose up -d --build flask-app"
             }
        }
    }
    post{
        success{
          emailext (subject: "CI/Cd pipleline msg",
                    body: "Code pipeline build successfully",
                    to : "aadvikpanchal358@gmail.com"
                    )
        }
        failure{
            
             emailext (subject: "CI/Cd pipleline msg",
                    body: "Code pipeline failed",
                    to : "aadvikpanchal358@gmail.com"
                    )
            
        }
    }
}
