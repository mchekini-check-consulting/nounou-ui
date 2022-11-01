node("ci-node") {
    stage("checkout") {
        checkout([$class: 'GitSCM', branches: [[name: '*/develop']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mchekini-check-consulting/nounou-ui.git']]])
    }
    stage("build") {
        sh "npm install"
        sh "ng build --configuration production"
    }
    stage("build docker image") {
        sh "sudo docker build -t nounou ."
    }
    stage("push docker image") {
        sh "sudo docker login -u mchekini -p jskabyliE"
        sh "sudo docker tag nounou mchekini/nounou:1.0"
        sh "sudo docker push mchekini/nounou:1.0"
    }
    node("integration-node") {
        stage("deploy nounou ui") {
            try {
                sh "sudo docker stop nounou"
                sh "sudo docker rm nounou"
                sh "sudo docker rmi mchekini/nounou:1.0"
            }catch(Exception e){
               println "No container nounou running"
            }
          sh "sudo docker run -p 80:80 --name nounou -d mchekini/nounou:1.0"
        }
    }

}
