node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        
        git url:'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git', branch: 'master'
    }    
    
    stage('Maven clean Package'){
        def mavenHome= tool name: 'Maven',type: "maven"
         sh "${mavenHome}/bin/mvn clean package"
         
    }
    
    stage('Build Docker Image'){
  
        sh "docker build -t sapnat/java_docker:${buildNumber} ."
    }
    stage('Docker Login And Push'){
        withCredentials([string(credentialsId: 'java_docker', variable: 'java_docker')]) {
            sh "docker login -u sapnat -p ${java_docker}"
        } 
        sh "docker push sapnat/java_docker:${buildNumber}"
    }
    
      stage('Deploy Application as Dokcer Container In Docker Deployment Server'){
        sshagent(['java_docker_ssh']) {
            sh "ssh -o StrictHostkeyChecking=no ubuntu@172.31.11.130 docker rm -f javawebappcontainer || true"
            
            
            sh "ssh -o StrictHostkeyChecking=no ubuntu@172.31.11.130 docker run -d -p 8080:8080 --name javawebappcontainer sapnat/java_docker:${buildNumber}"  
        }
      
    }
       
}
 
 
    
