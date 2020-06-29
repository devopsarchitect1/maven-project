node  {
   def mvnHome
   stage('Prepare') {
      git url: 'https://github.com/devopsarchitect/maven-project.git', branch: 'master'
      mvnHome = tool 'maven'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
      }
   }
  
   stage('Deploy') {
      // sh 'curl -u jenkins:jenkins -T target/**.war "http://192.168.1.118:8091/manager/text/deploy?path=/webapp&update=true"'
   
       
        deploy adapters: [tomcat8(credentialsId: 'eba0bcab-cfb7-4d2a-8197-7d339cd27e5d', path: '', url: 'http://192.168.1.118:8091')], contextPath: null, war: '**/*.war'
   }
   stage("Smoke Test"){
       sh "curl --retry-delay 10 --retry 5 http://localhost:8091/webapp"
   }
}
