pipeline{
    
    agent any
    
    stages {
        
            stage ('Initialize') {
            steps {
            sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
        }
            }
            stage('checkout scm') {
            steps {
              git credentialsId: 'Git Password', url: 'https://github.com/sonalikteli/webapp.git'
            }
            }

            stage('built'){
            steps{
            sh 'mvn package'
            }
            }

            stage ('Deploy-To-Tomcat-ssh') {
            steps {
               sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/devsecops-pipeline/target/*.war root@54.226.29.26:/opt/apache-tomcat/webapps/webapp.war'
           }       
      }


    }
}  
}