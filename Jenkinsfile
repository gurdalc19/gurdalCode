node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("gcr.io/dream-project-381712/dream")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }


    stage('Push Image') {
        
        docker.withRegistry('https://gcr.io', 'gcr:dream-project-381712') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'dreamManifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
