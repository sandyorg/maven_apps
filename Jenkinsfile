node {
    stage('Code Checkout') { // for display purposes
     echo 'Checout Code and clone it inside jenkins workspace.'
     git 'https://github.com/sandyorg/maven_apps.git'
   }
   stage('Build code') {
      echo 'Build the package'
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.6.1') {
       sh 'mvn clean compile'
     }
   }
   stage('SonarScan') {
      //withSonarQubeEnv('SonarQube') {
         withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.6.1') {
             sh 'mvn clean package sonar:sonar' 
             sh 'mvn org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar ' 
             ' -Dsonar.host.url=https://sonarcloud.io' +
             ' -Dsonar.organization=itrainsonar2 '+ 
             ' -Dsonar.login=f01b35533f7e199c643b56eea9031d9640d0a928 '   
         //}
      }
   }
    
   stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
    }  
   stage('Artifacts') {
       echo 'package the project artifacts..'
       withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.6.1') {
       sh 'mvn package'
     }
   
   }
   stage('Deploy to Dev'){
       echo 'Deploy to Dev environment'
   }
   stage('Deploy to Test'){
       echo 'Deploy to Test environment'
   }
   stage('Deploy to Prod'){
       echo 'Deploy to Prod environment'
   }
      
   
}
