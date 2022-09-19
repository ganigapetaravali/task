pipeline {
    environment {
      registry = "18.212.25.74:8001/repository/k8s-task/"
      registryCredential = 'nexus'
      dockerImage = ''
      SCANNER_HOME = tool 'sonarscanner'
      }
    agent any
    stages {
      stage('Cloning Git') {
        steps {
          git branch: 'main', url: 'https://github.com/ganigapetaravali/task.git'
        }
      }
      stage('Building image') {
        steps{
          script {
            sh 'docker build -t flask:6.0 .'
          }
         }
        }
      stage('Deploy Image in to nexus registry') {
        steps{
          script {
         //sh 'curl "admin:ravali" -X PUT http://18.212.25.74:8001/repository/k8s-task/flask:5.0 '
          //flask:3.0.push("latest")
            sh 'docker tag flask:3.0 18.212.25.74:8001/repository/k8s-task/flask:6.0'
            //sh 'docker login -u ravali1505 -p Manoj@123@123'
            sh 'docker login -u admin -p ravali 18.212.25.74:8001/repository/k8s-task/' 
            sh 'docker push 18.212.25.74:8001/repository/k8s-task/flask:6.0'
            sh 'docker logout http://18.212.25.74:8001/repository/k8s-task/'
              }
            }
         }
      stage('Sonarqube') {
        environment {
            scannerHome = tool 'sonarscanner'
    }
    steps {
        withSonarQubeEnv('productionsonarqubescanner') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        //timeout(time: 2, unit: 'MINUTES') {
        //    waitForQualityGate abortPipeline: true
        //}
     }
   }
        // integrated test cases
        stage('selinium-test') {
            steps {
               sh 'python hello.py'
            }
        }
  }  
}
