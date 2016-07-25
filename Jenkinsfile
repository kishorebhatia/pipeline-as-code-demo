#!groovy
jettyUrl = 'http://localhost:8081/'

node ('docker-cloud') {
   // Mark the code checkout 'stage'....
   stage 'Checkout'

   // Get some code from a GitHub repository
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/kishorebhatia/pipeline-as-code-demo']]])

   // Get the maven tool.
   // ** NOTE: This 'M3' maven tool must be configured
   // **       in the global configuration.           
   def mvnHome = tool 'Maven 3.x'

   // Mark the code build 'stage'....
   stage 'Build'
   // Run the maven build
   sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore clean package"
  // step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
   
   stage 'Approve'
   def email = "Please <a href=\""+ env.BUILD_URL + "/input/\">Approve</a> Jenkins job: " + env.JOB_NAME + " Build #" + env.BUILD_NUMBER 
mail bcc: '', body: email, cc: '', charset: 'UTF-8', from: 'beedemo.sa@gmail.com', mimeType: 'text/html', replyTo: 'beedemo.sa@gmail.com', subject: 'Please Approve', to: 'kbhatia@cloudbees.com'
input message: 'Approve?', submitter: 'kbhatia'
echo 'email sent'

stage 'DeployPROD'
echo 'Deployed to PROD'
}

def deploy(id) {
    unstash 'war'
    sh "cp x.war /tmp/webapps/${id}.war"
}

def undeploy(id) {
    sh "rm /tmp/webapps/${id}.war"
}

def runWithServer(body) {
    def id = UUID.randomUUID().toString()
    deploy id
    try {
        body.call "${jettyUrl}${id}/"
    } finally {
        undeploy id
    }
}*/
