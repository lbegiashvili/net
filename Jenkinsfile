node {
    def app
    //-------------------------------------
    stage('Clean up old images') {
        echo 'Cleaning up'
        sh 'docker rm -f $(docker ps -q -a) && docker rmi -f $(docker images -a -q) > /dev/null 2>&1' /* clean up dockerfile images*/
          
        
    }    
    //--------------------------------------    
    
    stage('Clone repository') {
      

        checkout scm
    }

    
    stage('Build image') {
  
       app = docker.build("lbegiashvili/test")
    }

    //stage('Test image') {
    //    app.inside {
    //        sh 'echo "Tests passed"'
    //    }
    // }
    // 

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatenetmanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
        
}
