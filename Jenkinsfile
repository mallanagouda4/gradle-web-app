node{
    def gradle_Home = tool name: 'gradle-5.6', type: 'gradle' 
     def gradlecmd = "${gradle_Home}/bin/gradle" 
     //def GradleHome = tool name: 'gradle-5.6', type: 'gradle'
      //def gradleCMD = "${GradleHome}/bin/gradle"
    stage('gitcheckout'){
        
        git credentialsId: '5e7cfcb2-a74a-485d-9e74-168e3683b0ea', url: 'https://github.com/mallanagouda4/gradle-web-app.git'
    }
    
    stage('gradlecleanbuild'){
        
        
      //sh "${gradleCMD} clean build"
        
        sh "${gradlecmd} clean build"
    }
    

stage('dockerbuild'){
    
    sh 'docker build -t ravikumara004/gradle-web-app .'
   }
   
   stage('imagepush'){
       withCredentials([string(credentialsId: 'docker', variable: 'hub_pass')]) {
    // some block
    sh 'docker login -u ravikumara004 -p ${hub_pass}'
    }
       
       
       sh 'docker push ravikumara004/gradle-web-app'
   

    }
    
    stage('deployment'){
        
         def dockerRun = 'docker run -d -p 8080:8080 --name gradle-web-app ravikumara004/gradle-web-app'
        sshagent(['pemkey']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.36.223 docker stop gradle-web-app || true'
          sh 'ssh  ubuntu@172.31.36.223 docker rm gradle-web-app || true'
          sh 'ssh  ubuntu@172.31.36.223 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.36.223 ${dockerRun}"
           
        }
    }
