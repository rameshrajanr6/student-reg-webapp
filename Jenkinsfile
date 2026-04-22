node {
    def Mavenhome = tool name: 'maven3.9.14', type:'maven'
    
  stage("Stage One"){
    git branch: 'main', changelog: false, credentialsId: '9d986298-8b4c-4865-8d7b-ecfeabf6673b', poll: false, url: 'https://github.com/rameshrajanr6/student-reg-webapp.git'
  }
  stage("Maven Build"){
    sh "${Mavenhome}/bin/mvn clean package"     
   }
   stage("Sonar Scan"){
       withCredentials([string(credentialsId: 'sonar', variable: 'sonarToken')]) {
         sh "${Mavenhome}/bin/mvn clean verify sonar:sonar"
       }
   }
   stage("Nexus Upload"){
     sh "${Mavenhome}/bin/mvn clean deploy"   
   }
   stage("Deploy to container"){
     sshagent(['ssh-user'])  {
        sh "scp -o StrictHostKeyChecking=no target/student-reg-webapp.war ubuntu@13.218.51.11:/opt/tomcat/webapps"
         }
      }
   }
