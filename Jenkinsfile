pipeline {
    agent any
    stages {
        stage('init') {
            steps {
                script {
                    name = "e-clent"
                    port = "80:80"
                    gatewayPort = "9090:9090"
                    registry = "master:5000"
                    buildNumber = "1.0.$BUILD_NUMBER"
                }
            }
        }
        stage('fetch') {
            steps {
                git url: 'https://github.com/mmahu/client.git', branch: 'master'
            }
        }
        stage('imaging') {
            steps {
                sh "docker build . -t ${registry}/${name}:${buildNumber}"
                sh "docker push ${registry}/${name}"
            }
        }
        stage('deploy') {
            steps {
                sh "docker service rm ${name} || true"
                sh "docker service create \
                    --name ${name} \
                    --no-resolve-image \
                    --publish ${port} \
                    --publish ${gatewayPort} \
                    ${registry}/${name}:${buildNumber}"
            }
        }
    }
}