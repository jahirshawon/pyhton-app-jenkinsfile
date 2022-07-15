node{
   stage('SCM Checkout'){
       git 'https://github.com/jahirshawon/python-app-jenkinsfile'
   }
   stage('Test Code'){
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
   stage('Deploy on Staging'){ 
     def dockerRun = 'docker run  -p 6379:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4040:80 -d --link redis --name my-python-app jahirshawon/my-testpython:2.0.0'
     def dockerRun2 = 'docker rm -f my-pyhton-app'
     sshagent(['dockerserver4']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun1}"
   }
   stage('Deploy on Production'){ 
     def dockerRun3 = 'docker run  -p 6379:6379 -d --name redis redis'
     def dockerRun4 = 'docker run -p 4040:80 -d --link redis --name my-python-app jahirshawon/my-testpython:2.0.0'
     def dockerRun5 = 'docker rm -f my-pyhton-app'
     sshagent(['dockerserverprod']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.241 ${dockerRun3}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.241 ${dockerRun5}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.241 ${dockerRun4}"
     }
     }
   }
}
