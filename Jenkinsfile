pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage started'
                sh '''
                mkdir -p build
                echo "Build completed successfully" > build/build.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage started'
                sh '''
                mkdir -p reports
                echo "All tests passed" > reports/test-report.txt
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging artifact'
                sh '''
                mkdir -p target
                tar -czf target/ci-artifact.tar.gz build reports
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.tar.gz'
            }
        }
    }

    post {
        success {
            echo 'CI job completed successfully'
        }
        failure {
            echo 'CI job failed'
        }
        always {
            cleanWs()
        }
    }
}
