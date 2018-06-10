pipeline {
    agent any
    tools {
        jdk 'java1.8'
        maven 'maven3'
    }
    stages {
        stage('Initialize') {
            steps {
                echo "This is initialize step"
                sh '''
                    echo "PATH=${PATH}"
                    echo "M2_HOME=${M2_HOME}"
                '''
            }
        }
        stage('Build') {
            steps {
                echo "Build is going on"
                sh 'mvn clean package checkstyle:checkstyle'
            }
            post {
                success {
                    echo "Generate checkstyle report"
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                    echo "Generate Junit report"
                    junit '*/target/surefire-reports/*.xml'
                    echo "Archieve Artifact"
                    archiveArtifacts '**/*.war'
                }
            }
        }
        stage('Deploy_to_staging'){
            steps {
                echo "Now build is deploying to staging env"
                build 'deploy-staging'
            }
        }
        stage('Deploy_to_production'){
            steps {
                timeout(2) {
                            input 'Do you want to proceed now ?'
                            }
                echo "Now build is deploying to production env"
                build 'deploy-prod'
            }
        }
    }
}