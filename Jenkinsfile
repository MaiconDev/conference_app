pipeline {
    environment {
        registry = "562197769032.dkr.ecr.us-east-2.amazonaws.com/conference_app"
        profile = "dev"
        //BUILD_NUMBER = sh (returnStdout: true, script: "git tag --sort version:refname | tail -1").trim()
    }

    agent any

    stages {

        stage('Building our image') {
            steps{


                //sh "echo BUILD_NUMBER=$(git tag --sort version:refname | tail -1) > env.properties"
                sh '''export BUILD_NUMBER=$(git tag --sort version:refname | tail -1)'''

                sh "echo MAICON"
                sh "echo $BUILD_NUMBER"

                //--rm
                sh '''
                    docker run -i --name mvn-build-java -v $(pwd):/api -w /api maven:3.6.1-jdk-13-alpine mvn clean install -DskipTests;
                    mv ./target/start.jar ./docker/production
                '''

                script {
                    docker.build(registry + ":" + BUILD_NUMBER, "./docker/production")
                }
            }
        }
        stage('Deploy image') {
            steps{
                // script {
                //     //This step should not normally be used in your script. Consult the inline help for details.
                //     withDockerRegistry(credentialsId: registryCredential, url: registryUrl) {
                //         docker.image(registry + ":" + BUILD_NUMBER).push(BUILD_NUMBER)
                //     }
                // }

//                 sh '''
//                 echo "[profile dev]
// aws_access_key_id=*********
// aws_secret_access_key=*********************
// region=us-east-2" > ~/.aws/config;
//                 '''

                sh "aws ecr get-login-password --profile ${profile} | docker login --username AWS --password-stdin ${registry}"
                sh "docker push ${registry}:${BUILD_NUMBER}"
            }
        }
        stage('Cleaning up') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}

Spring Boot project with Spring Data and Spring MVC. Containerized build with docker.
conference_app