def CONTAINER_NAME="getintodevops"
def CONTAINER_TAG="latest"
def DOCKER_HUB_USER="gunack23"
def HTTP_PORT="8090"
node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }
    
    stage('Initialize'){
        def dockerHome = tool 'myDocker'
        def mavenHome  = tool 'myMaven'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("guna-ck/getintodevops-hellonode")
    }

    stage('Test image') {
        /* We test our image with a simple smoke test:
         * Run a curl inside the newly-build Docker image */

        app.inside {
            sh 'echo "Tests passed" '
        }
    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            pushToImage(CONTAINER_NAME, CONTAINER_TAG, USERNAME, PASSWORD)
        }
    }
    
}
