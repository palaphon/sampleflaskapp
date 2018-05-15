node {
    dir("/root/"){
    checkout scm

    env.DOCKER_API_VERSION="1.23"
    appName = "default/flask-app"
    registryHost = "172.12.34.20:8500/"
    imageName = "${registryHost}${appName}:${env.BUILD_ID}"
    env.BUILDIMG=imageName
    docker.withRegistry('http://172.12.34.20:8500/', 'docker'){
    stage "Build"

        def pcImg = docker.build("172.12.34.20:8500/default/flask-app:${env.BUILD_ID}", "-f Dockerfile .")
        sh "cp /var/jenkins_home/.dockercfg ${HOME}/.dockercfg"
        pcImg.push()

    /**
    input 'Do you want to proceed with Deployment?'
    **/
    stage "Deploy"

        sh "kubectl set image deployment/demoapp-demochart demochart=${imageName}"
        sh "kubectl rollout status deployment/demoapp-demochart"
}
}
}
