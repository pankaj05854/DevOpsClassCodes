node{

    stage('SCM  Checkout'){
            git credentialsId: 'jenkins-credentials', url: 'https://github.com/pankaj05854/DevOpsClassCodes.git'
    }
    stage('Compile'){
        def mvnHome = tool name: 'maven3.6', type: 'maven'
        def mvnCmd = "${mvnHome}/bin/mvn"
        sh "${mvnCmd} clean compile"
    }
    stage("Unit test"){
        def mvnHome = tool name: 'maven3.6', type: 'maven'
        def mvnCmd = "${mvnHome}/bin/mvn"
        sh "${mvnCmd} test"           
    }
    stage('SonarQube Analysis'){

        withSonarQubeEnv('sonarqube-5.6') {
        def mvnHome = tool name: 'maven3.6', type: 'maven'
        def mvnCmd = "${mvnHome}/bin/mvn"
        sh "${mvnCmd} clean package sonar:sonar"
            }
        }
        
    stage("Quality check PASS/FAIL"){
        timeout(time: 1, unit: 'MINUTES') {
        def qg = waitForQualityGate()
        if (qg.status != 'OK') {
            error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
          }
      } 
      
    state('Deply to Tomcat'){
        sshagent(['tomcatserver-dev']){
            sh 'scp -o StrickHostKeyChecking=no target/*.war ec2-user@IP:/opt/tomcat/webapps/'
        }
    }
    }
