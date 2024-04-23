pipeline {
    agent any

    stages {
        stage("Build docker image"){
            steps{
                script {
                    sh """
                        docker build -t spring-petclinic -f Dockerfile_B . > logs_build_${env.BUILD_NUMBER}.log 2>&1 || exit 1
                    """
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: "logs_build_${env.BUILD_NUMBER}.log", fingerprint: true
                }
            }
        }
        stage("Test"){
            steps{
                script {
                    sh """
                        docker build -t test -f Dockerfile_T . > logs_test_${env.BUILD_NUMBER}.log 2>&1 || exit 1
                    """
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: "logs_test_${env.BUILD_NUMBER}.log", fingerprint: true
                    archiveArtifacts artifacts: "test_results_${env.BUILD_NUMBER}.log", fingerprint: true
                }
            }
        }
    }
    post {
        always {
            sh """
                docker rmi spring-petclinic test
            """
        }
    }
}
