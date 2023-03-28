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
        stage('Build') {
            steps {
                sh "echo ${GCR_CRED} > login.json"
                sh 'docker login -u _json_key -p "$(cat login.json)" https://gcr.io'
                sh "docker image ls"
                sh "docker push --all-tags ${GCR_REPO}"

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
