pipeline {
    agent any

    environment {
        GCR_CRED = credentials('jenkins-cred')
        GCR_REPO = "gcr.io/dream-project-381712"
        IMAGE_TAG = "${env.BUILD_ID}"
    }
    stages {
        stage('Clone repository') {
                steps{
                    checkout scm
                }
    }
    stage("docker build"){
        Img = docker.build(
            "dream-project-381712/dream:$IMAGE_TAG",
            "-f Dockerfile ."
        )
    }

    stage("docker push") {
        docker.withRegistry('https://gcr.io', "gcr:dream-project-381712") {
            Img.push("$IMAGE_TAG")
        }
}
        }
    }
}
