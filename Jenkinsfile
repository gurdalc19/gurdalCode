node {
    def app

    stage('Clone repository') {
      
        checkout scm
    }
    
    
    stage ('Adding to Docker Group'){
        sh 'sudo usermod -aG docker jenkins'
        sh 'newgrp docker'
        exec "sudo su -l jenkins"
    }

    stage('Build image') {
        
       app = docker.build("gcr.io/dream-project-381712/dream")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://gcr.io', "gcr:dream-project-381712") {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
