node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "maven${env.BUILD_NUMBER}"

    stage('Clone Repo') {
        git 'https://github.com/shyamyadav043/maven-project.git'
    }    

    stage('Build Project') {
        sh "'${mvnHome}/bin/mvn' clean install"
    }

    stage('Build Docker Image') {
        dockerImage = docker.build("maven:${env.BUILD_NUMBER}")
    }

    stage('Docker Login') {
        echo "Docker Image Tag: ${dockerImageTag}"
        sh "docker images"
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
        }
    }

    stage('Docker Tag and Push') {
        sh "docker tag maven:${env.BUILD_NUMBER} shyam043/mavencicd:${env.BUILD_NUMBER}"
        sh "docker push shyam043/mavencicd:${env.BUILD_NUMBER}"
    }
}
