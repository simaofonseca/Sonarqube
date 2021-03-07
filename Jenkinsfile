pipeline
{
	agent any

	parameters
	{
		string(name: 'DOCKER_IMAGE', defaultValue: 'default_name', description: 'Docker image')

	}

    stages
    {
        stage('SonarQube') {
            steps {
		    script {
			def scannerHome = tool 'sonarqube';
              		withSonarQubeEnv('sonarqube') {
		      		sh "${sonarqube}/bin/sonar-scanner \
				-D sonar.login=4699207e825f7cd9d9037819d3fc74d7cf380d29 \
				-D sonar.projectKey=jenkins \
				-D sonar.java.binaries=/var/jenkins_home/workspace/Sonarqube \
				-D sonar.java.source=11 \
				-D sonar.host.url=http://sonarqube:9000"
					}
		    }
          }
        }
        stage('Docker build'){
            steps
            {
		sh "docker rmi -f ${DOCKER_IMAGE}"
		sh "docker build -t ${DOCKER_IMAGE} ."
                sh "docker login -u admin -p admin localhost:8082 "
		sh "docker tag ${DOCKER_IMAGE} localhost:8082/${DOCKER_IMAGE}"
		sh "docker push localhost:8082/${DOCKER_IMAGE}"
                sh "javac *.java"
                sh "jar cfe calculator.jar Calculadora2 ./*.class"
                sh "curl -v -u 'admin:admin' --upload calculator.jar http://nexus:8081/repository/my-raw/"

            }
        }
    }
}
