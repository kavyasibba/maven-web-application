node{
    def mavenHome = tool name: "maven 3.8.4"
    
    stage('CheckoutCode'){
        git branch: 'development', credentialsId: '9d3ecac3-27e1-44cb-a068-2e36d5c0cbf9', url: 'https://github.com/kavyasibba/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean package sonar:sonar"
    }
    stage('UploadingArtficatsIntoNexus'){
        sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
    }
    stage('DeployingToTomcat'){
        sshagent(['87d2a06b-a94c-4540-b954-f51394359645']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.66.245.56:/opt/apache-tomcat-9.0.54/webapps/"
         }
    }
    stage('Emailnotifiction'){
        emailext body: '''build over

Regards
kavya.''', subject: 'Build', to: 'kavya.sibbala@gmail.com'
    }
}
