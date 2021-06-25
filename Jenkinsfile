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

            stage ('Deploy-To-Tomcat') {
            steps {
               sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war root@18.213.110.23:/opt/apache-tomcat/webapps/webapp.war'
           }       
      }


    }
}  
}