pipeline {
    agent any
    environment {
        DOCKER_TAG=dockertag()
    }
    stages {
        stage("Build Docker Image")
            steps {
                sh "docker build . -t ${DOCKER_TAG}"
            }
    }
}

    def dockertag() {
        def tag = sh script: "git rev-parse HEAD", returnStdout: true
        return tag
    }
