pipeline {
    agent any
    stages {
        stage('init') {
            steps {
                script {
                    name = "e-client"
                    port = "80:80"
                    registry = "192.168.0.25:5000"
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
                    --publish ${port} \
                    ${registry}/${name}:${buildNumber}"
            }
        }
    }
}