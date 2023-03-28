pipeline {
    agent any
    environment {
        GCR_CRED = credentials('dream-project-381712')
        GCR_REPO = "gcr.io/dream-project-381712"
        IMAGE_TAG = "${env.BUILD_ID}"
    }
    stages {
        stage('Clone repository') {
                steps{
                        checkout scm
                }
    }
        stage('Build') {
            steps {
                echo $GCR_CRED > login.json
                cat login.json | docker login cat jenkins-sa.json | docker login -u _json_key --password-stdin 'https://gcr.io'
                docker build . -t "${GCR_REPO}":"${IMAGE_TAG}"
                docker image ls
                docker push --all-tags $GCR_REPO

            }
        }
        stage('Trigger ManifestUpdate') {
            steps{
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
            }
        }
    }
}
