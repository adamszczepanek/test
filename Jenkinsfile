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
        string(name: 'DOCKER_PWD', defaultValue: '', description: '')
    }    
    environment {
        REPONAME = "adamszczepanek/test"
    }   
    stages {
        stage("Build image") {
            steps {
                sh "docker build -t ${REPONAME}:1 ."
            }
        }
        stage("Docker Login") {
            steps {
                sh "docker login --username adamszczepanek --password ${DOCKER_PWD}"
            }
        }   
        stage("Docker Cleanup") {
            steps {
                sh "docker rmi ${REPONAME}:1"
            }
        }
        
    }
}
