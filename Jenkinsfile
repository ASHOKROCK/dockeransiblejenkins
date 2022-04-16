pipeline{
    agent any
    
    tools {
     maven 'MAVEN'
    }
     
     stages{
        
        stage('CHECKOUT SCM CODE'){
            steps{
                 git 'https://github.com/SOFTWARESOLUTONS-PVT-LIMITED/KCMavenWebProject.git'
            }
        }
        
        stage('BUILD PACKAGE'){
            steps{
              sh "mvn clean package"
            }
        }
        
        stage('EXECUTE SONARQUBE ANALYSIS'){
            steps{
               withSonarQubeEnv(credentialsId: 'SONAR_ID',installationName:'sonarqube-server') {
                 sh "mvn clean package sonar:sonar"
              }
                
            }
        }
        
        stage('ARTIFACT INTO NEXUS'){
            steps{
                nexusArtifactUploader artifacts: 
             [
                [
               artifactId: 'maven-web-project', 
                classifier: 'debug', 
                file: '/var/lib/jenkins/workspace/devopsnew-smart/target/maven-web-project-5.0-SNAPSHOT.war', 
                type: 'war'
              ]
              ],
             credentialsId: 'NEXUS_ID',
             groupId: 'com.kctechnologies',
             nexusUrl: '35.240.144.33:8081', 
              nexusVersion: 'nexus3', 
             protocol: 'http', 
              repository: 'maven2-host-rele', 
             version: '5.0-SNAPSHOT'
               
                
                    
            }
        }
        
        stage('DEPLOY TO TOMCAT'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'TOMCAT_PWD', path: '', url: 'http://34.118.96.111:8085/')], contextPath: 'maven-web-project', war: 'target/*.war'
            }
        }
        
        stage('SEND EMAIL NOTIFICATION'){
            steps{
         mail bcc: 'sappoguashokrock6629@gmail.com', body: '''REGARDS
            MR S.ASHOKKUMAR
            ROLE-DEVOPSENGINEER
            PROJECTTYPE-JAVABASEDPROJECT
          ''', cc: 'cdeccmogtsrowvul', from: '', replyTo: '', subject: 'BUILD IS OVER', to: 'sappoguashok462@gmail.com'
        }
            
        }
        
        stage('DOKCER IMAGE'){
            steps{
               sh "docker build -t mysmartnewcustom ."
            }
        }
        
        stage('DOCKER LOGIN AND PUSH'){
            steps{
                withCredentials([string(credentialsId: 'DOCKER-HUB-PWD', variable: 'DOKCERHUB')]) {
                  sh "docker login -u ashokganidocker -p ${DOKCERHUB}"
            }
            sh "docker tag  mysmartnewcustom  ashokganidocker/mysmartnewcustom:latest"
            sh "docker push ashokganidocker/mysmartnewcustom:latest"
        }
        }
        
        stage('DOCKER RUN'){
            steps{
                sh "docker run -d --name webserver-6 -p 8096:80 mysmartnewcustom" 
            }
        }
        
        stage ('SEND SLACK NOTIFCATION'){
            steps{
               slackSend channel: '#newbuildnotfcation', color: 'good', message: 'Welcome to Jenkins, Slack!', teamDomain: 'DEVTEAM', tokenCredentialId: 'SLACK_TOKEN'
            }
        }
         
         
     }//satgesclosed
}//pipelineclosed
