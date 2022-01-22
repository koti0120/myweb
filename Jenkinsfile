pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'f5799938-5221-4fb1-8415-7542f5cd5780', url: 'https://github.com/koti0120/myweb.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.42.115:/home/ec2-user/apache-tomcat-9.0.56/webapps/
                    
                    ssh ec2-user@172.31.42.115 /home/ec2-user/apache-tomcat-9.0.56/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.42.115 /home/ec2-user/apache-tomcat-9.0.56/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
