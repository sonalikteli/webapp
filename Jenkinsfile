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
            stage('git checkout') {
            steps {
              git credentialsId: 'Git Password', url: 'https://github.com/sonalikteli/webapp.git'
            }
            }

            stage('Check-Git-Secrets') {
              steps {
                sh 'rm trufflehog || true'
                sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
                sh 'cat trufflehog'
              }
            }

            stage ('Source Composition Analysis') {
              steps {
              sh 'rm owasp* || true'
              sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
              sh 'chmod +x owasp-dependency-check.sh'
              sh 'bash owasp-dependency-check.sh'
              sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
           stage ('SAST') {
           steps {
                withSonarQubeEnv('sonar') {
                sh 'mvn sonar:sonar'
                sh 'cat target/sonar/report-task.txt'
        }
      }
    }
                  
               
            
            stage('built'){
            steps{
            sh 'mvn package'
            }
            }


            stage('Nexus'){
              steps {
                nexusArtifactUploader artifacts: [[artifactId: 'junit', classifier: '', file: 'target/WebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'junit', nexusUrl: 'http://54.173.206.92:8081/nexus/', nexusVersion: 'nexus2', protocol: 'http', repository: 'Releases', version: '3.8.1'
                
              }
            }
            stage ('Deploy-To-Tomcat-ssh') {
            steps {
<<<<<<< HEAD
              sshagent(['tomcatt']) {
                sh 'scp -o  /var/lib/jenkins/workspace/devsecops-pipeline/target/*.war ec2-user@54.160.109.16:/opt/apache-tomcat/webapps/webapp.war'
=======
              sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/devsecops-pipeline/target/*.war ec2-user@54.160.109.16:/opt/apache-tomcat/webapps/webapp.war'
>>>>>>> ae60a5adfcdbfa70e4bdc2b3d8c11030fbe8f3bd
           }       
      }
          stage('Docker Build Image') {
            steps {
              sh 'docker build -t sonali224/mywebapp:${DOCKER_TAG} .'
              sh 'docker images'
            }
          }
          
          stage('Docker hub') {
            steps {
              withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
    
}
            }
          }

    }
}  
}
