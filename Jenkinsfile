node{
   stage('SCM Checkout'){
       git 'https://github.com/jahirshawon/python-app-jenkinsfile'
   }
   stage('check git files'){
     sh 'ls'
   }
   stage('build Docker Image'){
     sh 'docker build -t jahirshawon/my-testpython:2.0.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u jahirshawon -p ${dockerhubpassword}"
     }
     sh 'docker push jahirshawon/my-testpython:2.0.0'
   }
   stage('Run Container on Dev Server'){ 
     def dockerRun = 'docker run -p 5000:5000 -d --name my-python-app jahirshawon/my-testpython:2.0.0'
     def dockerRun1 = 'docker rm -f my-pyhton-app'
     sshagent(['dockerserver4']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun1}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun}"
     }
   }
}
