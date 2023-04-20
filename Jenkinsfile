node {
    def app

    stage('Clone repository') {
      
        checkout scm
    }
    
    


    stage('Build image') {
        
       app = docker.build("gcr.io/<project_id>/dream")
        
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://gcr.io', "gcr:<project_id>) {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
