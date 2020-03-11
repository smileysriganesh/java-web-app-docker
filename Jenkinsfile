node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/sriganesh-git/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t smileysri/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u smileysri -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push smileysri/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app smileysri/java-web-app'
         
         sshagent(['Docker_Dev_Server_SSH']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.226 docker stop java-web-app-docker || true'
          sh 'ssh  ubuntu@172.31.11.226 docker rm java-web-app-docker || true'
          sh 'ssh  ubuntu@172.31.11.226 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.11.226 ${dockerRun}"
       }
       
    }
     
     
}
