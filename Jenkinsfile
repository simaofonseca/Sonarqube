pipeline {
    agent any

            stages {
                stage('Clone') {
                    steps {
                        git branch: 'main', url: 'https://github.com/simaofonseca/Sonarqube.git'
                    }
                }
                stage ('Criar .jar') {
                    steps {
                        sh 'javac *.java'
                        sh 'jar cfe calc.jar ./Calculadora-Sonar *.class'
                    }   
                }
                stage('SonarQube analysis') {
                        environment { scannerHome = tool 'sonarqube' }
                    steps {
                            withSonarQubeEnv('sonarqube') {
                                    sh "${scannerHome}/bin/sonar-scanner \
                                    -D sonar.login=a71522f0b9f9d3ddebf155f41e5ce23561b4c222 \
                                    -D sonar.projectKey=sonarqube \
                                    -D sonar.java.binaries=/var/jenkins_home/workspace/Sonarqube \
                                    -D sonar.java.source=11 \
                                    -D sonar.host.url=http://localhost:9000/"
                            }
                        }
                    }

            }
}
