node('new43') {
// Delete the workspace
//deleteDir()
stage('Retrieve source code') {
    checkout scm
    delivery = load 'repository.groovy'
    sh " cd $WORKSPACE;/bin/mkdir Build-${env.BUILD_NUMBER} "
    }
try {
    stage('Maven Build') {
      docker.image('maven:3.5-jdk-8-alpine').inside {
        sh "mvn clean package -Dbuild.number=${BUILD_NUMBER}"
        sh "/bin/mv -f $WORKSPACE/target/*.war $WORKSPACE/Build-${env.BUILD_NUMBER}/application123_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war"
       }
    }
   stage('Deploy') {
        sh "/bin/cp -f $WORKSPACE/Build-${env.BUILD_NUMBER}/application123_${env.BRANCH_NAME}${env.BUILD_NUMBER}.war /root/apache-tomcat-8.5.37/webapps/application.war"
    }
  
   delivery.artifactory()
  }
  catch (e) {
      currentBuild.result = "FAILED" 
      throw e
    }
}
