node{
      def dockerImageName= 'afrozkhan94/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/afroz-khan94/JavaCICD-Project.git'
      }
      stage('Build'){
         // Get maven home path and build
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'
         //sh "${mvnHome}/bin/mvn package -Dmaven.test.skip=true"
         sh "sh /usr/bin/mvn package -Dmaven.test.skip=true"
      }

     stage ('Test'){
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'
         //sh "${mvnHome}/bin/mvn verify; sleep 3"
         sh "/opt/maven/bin/mvn verify; sleep 3"
      }

     stage('Build Docker Image'){
           sh "docker build -t ${dockerImageName} ."
      }

      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'DockerPwd', variable: 'DockerPwd')]) {
              sh "docker login -u afrozkhan94 -p ${DockerPwd}"
         }
        sh "docker push ${dockerImageName}"
      }

    stage('Run Docker Image'){
            def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            def changingPermission='sudo chmod +x stopscript.sh'
            def scriptRunner='sudo ./stopscript.sh'
            def dockerRun= "sudo docker run -p 8089:8090 -d --name ${dockerContainerName} ${dockerImageName}"
            withCredentials([string(credentialsId: 'deploymentpass', variable: 'dpPWD')]) {
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196"
                  //sh "sshpass -p ${dpPWD} scp -r stopscript.sh devops@52.76.172.196:/home/devops"
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${changingPermission}"
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${scriptRunner}"
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no devops@52.76.172.196 ${dockerRun}"
                    sh "${dockerRun}"
            }


      }


  }
