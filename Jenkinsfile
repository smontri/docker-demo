pipeline{
    agent any

    environment {
        IMAGE_TAG  = 'smontri/docker-demo'
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages{
        stage ('clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage ('checkout scm') {
            steps {
                git 'https://github.com/smontri/docker-demo.git'
            }
        }

        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=docker-demo \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=docker-demo '''
                }
            }
        }

        stage ('Build and push to docker hub'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t $IMAGE_TAG ./src/app/Dockerfile"
                        sh "docker push $IMAGE_TAG:latest"
                   }
                }
            }
        }

        stage('Analyze image') {
            steps {
                // Install Docker Scout
                sh 'curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s -- -b /usr/local/bin'

                script{
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                    // Analyze and fail on critical or high vulnerabilities
                    sh 'docker-scout cves $IMAGE_TAG --exit-code --only-severity critical,high'
                    }
                }
            }
        }

        stage ('Deploy app to container'){
            steps{
                sh 'docker run -d --name docker-demo -p 8080:8080 $IMAGE_TAG:latest'
            }
        }

   }
}