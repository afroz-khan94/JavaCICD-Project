node {
    def dockerImageName = "afrozkhan94/javadedockerapp_${JOB_NAME}:${BUILD_NUMBER}"
    def dockerContainerName = "javadockerapp_${JOB_NAME}_${BUILD_NUMBER}"
    def changingPermission = "sudo chmod +x stopscript.sh"
    def scriptRunner = "sudo ./stopscript.sh"
    def dockerRun = "sudo docker run -p 8089:8090 -d --name ${dockerContainerName} ${dockerImageName}"

    stage('SCM Checkout') {
        git 'https://github.com/afroz-khan94/JavaCICD-Project.git'
    }

    stage('Build') {
        // Use Jenkins tool configuration for Maven
        def mvnHome = tool name: 'Maven 3.8.7', type: 'maven'  // Adjust Maven version as per your configuration
        sh "${mvnHome}/bin/mvn package -Dmaven.test.skip=true"
    }

    stage('Test') {
        // Use Jenkins tool configuration for Maven
        def mvnHome = tool name: 'Maven 3.8.7', type: 'maven'  // Adjust Maven version as per your configuration
        sh "${mvnHome}/bin/mvn verify; sleep 3"
    }

    stage('Build Docker Image') {
        sh "docker build -t ${dockerImageName} ."
    }

    stage('Publish Docker Image') {
        withCredentials([string(credentialsId: 'DockerPwd', variable: 'DockerPwd')]) {
            sh "docker login -u afrozkhan94 -p ${DockerPwd}"
        }
        sh "docker push ${dockerImageName}"
    }

    stage('Run Docker Image') {
        withCredentials([string(credentialsId: 'deploymentpass', variable: 'dpPWD')]) {
            // If you want to use SSH to execute commands on a remote server, uncomment and configure the following:
            // sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196"
            // sh "sshpass -p ${dpPWD} scp -r stopscript.sh devops@52.76.172.196:/home/devops"
            // sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${changingPermission}"
            // sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${scriptRunner}"
            sh "${dockerRun}"
        }
    }
}
