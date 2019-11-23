pipeline {
    agent { 
        docker { 
            image 'golang' 
        }
    }
    environment {
        GOCACHE = "/tmp"
        docker_registry = "magalixcorp/k8scicd"

    }
    stages {
        stage('Build') {
            steps {
                // Create our project directory.
                sh 'cd ${GOPATH}/src'
                sh 'mkdir -p ${GOPATH}/src/hello-world'
                // Copy all files in our Jenkins workspace to our project directory.                
                sh 'cp -r ${WORKSPACE}/* ${GOPATH}/src/hello-world'
                // Build the app.
                sh 'go build'               
            }     
        }
        stage('Test') {
            steps {                 
                // Create our project directory.
                sh 'cd ${GOPATH}/src'
                sh 'mkdir -p ${GOPATH}/src/hello-world'
                // Copy all files in our Jenkins workspace to our project directory.                
                sh 'cp -r ${WORKSPACE}/* ${GOPATH}/src/hello-world'
                // Remove cached test results.
                sh 'go clean -cache'
                // Run Unit Tests.
                sh 'go test ./... -v -short'            
            }
        }
    }
}

pipeline {
    agent {
        dockerfile true
    }
    stage('Publish') {
            steps{
                script {
                    def appimage = docker.build("k8scicd:${env.BUILD_ID}")
                    appimage.push()
                    appimage.push('latest')
                }
            }
        }
}