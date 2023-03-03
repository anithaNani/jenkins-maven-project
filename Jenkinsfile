pipeline{
    agent any
    tools {
       maven 'maven'
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'javahome', url: 'https://github.com/anithaNani/jenkins-maven-project.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn -f hello-world/pom.xml -B -DskipTests clean package"
                sh "mv target/*.war target/myweb.war"
            } 
        }
        stage("deploy-dev"){
            steps{
                sshagent(['c8da5eaa-a965-4a8a-8df2-9da798ad43ca']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ubuntu@172.31.20.160:/home/ubuntu/apache-tomcat-9.0.72/webapps/
                    
                    ssh ubuntu@172.31.20.160:/home/ubuntu/apache-tomcat-9.0.72/bin/shutdown.sh
                    
                    ssh ubuntu@172.31.20.160:/home/ubuntu/apache-tomcat-9.0.72/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
