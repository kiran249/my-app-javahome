pipeline {
    agent any
    tools { 
        maven 'maven3' 
    }
    parameters {
        string(name: 'SSH_Ip_address', defaultValue: '35.171.25.143', description: 'Enter ssh Ip address')
        string(name: 'SSH_UserName', defaultValue: 'ec2-user', description: 'Enter ssh Username')
    }
    stages {
        stage('git clone') { 
            steps {
                git 'https://github.com/kiran249/my-app-javahome.git' 
            }
        }
        stage('maven install') { 
            steps {
                sh 'mvn clean install -DskipTests' 
            }
        }
        stage('ssh connections') { 
            steps {
                sshagent(['ssh_connection']) {
                sh "scp -o StrictHostKeyChecking=no target/*.war '${SSH_UserName}'@'${SSH_Ip_address}':/home/ec2-user/apache-tomcat-10.0.16/webapps/app.war"
                sh "ssh '${SSH_UserName}'@'${SSH_Ip_address}' 'sh /home/ec2-user/apache-tomcat-10.0.16/bin/shutdown.sh' "
                sh "ssh '${SSH_UserName}'@'${SSH_Ip_address}' 'sh /home/ec2-user/apache-tomcat-10.0.16/bin/startup.sh' "
            }
            }
        }
        
    }
}
