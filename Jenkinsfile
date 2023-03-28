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
                sh "cat login.json | docker login cat jenkins-sa.json | docker login -u _json_key --password-stdin 'https://gcr.io'"
                sh "docker build  -t ${GCR_REPO}:{$IMAGE_TAG} -t ${GCR_REPO} ."
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
