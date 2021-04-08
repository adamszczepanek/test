pipeline {
    agent any
    options {
        // keep only last 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10'))        // Disallow concurrent executions of the Pipeline.
        disableConcurrentBuilds()        // Timestamps
        timestamps()
        skipStagesAfterUnstable()
    }    
    parameters {
        choice(name: 'CHOICE_NODE_VERSION', choices: ['all','14.4.0','14.0.0','13.14.0','12.18.1'], description: 'Run on specific node version')
    }    
    environment {
        REPONAME = "valueaddpoland/ci-node"
    }   
    stages {
        stage("Build image") {
            steps {
                sh "docker build -t ${REPONAME}:${NODE_VERSION} --build-arg NODE_VER=${NODE_VERSION} ."
            }
        }        
        stage("Docker Tag & Push") {
            steps {
                withDockerRegistry([url: "", credentialsId: "valueadd-robot-dockerhub"]) {
                    sh "docker push ${REPONAME}:${NODE_VERSION}"
                }
            }
        }        
        stage("Docker cleanup") {
            steps {
                sh "docker rmi ${REPONAME}:${NODE_VERSION}"
            }
        }
    }
}
