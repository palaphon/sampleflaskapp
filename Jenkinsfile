node {
    
	dir("/root/"){
    
		checkout scm
		
		env.DOCKER_API_VERSION="1.23"

		appName = "default/flask-app"

		registryHost = "icpinetiknowplus.tk:8500/"

		imageName = "${registryHost}${appName}:4"

		env.BUILDIMG=imageName

		docker.withRegistry('https://icpinetiknowplus.tk:8500/', 'docker'){

			stage "Build"

				def pcImg = docker.build("icpinetiknowplus.tk:8500/default/flask-app:4", "-f Dockerfile .")

				sh "cp /var/jenkins_home/.dockercfg ${HOME}/.dockercfg"

				pcImg.push()

				input 'Do you want to proceed with Deployment?'

			stage "Deploy"

				sh "kubectl set image deployment/demoapp-demochart demochart=${imageName}"

				sh "kubectl rollout status deployment/demoapp-demochart"

		}
	}
}
