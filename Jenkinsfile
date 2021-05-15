node{
    stage("Git CheckOut"){
        git url: 'https://github.com/thippeswamy242/Jenkins-Docker.git',branch: 'main'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      }
	  
	  stage(" SonarQube analysis"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
	  withSonarQubeEnv('My SonarQube Server'){
      sh "${mavenCMD} sonar:sonar"
      }
	  }
	  
	  stage("Quality Gate"){
  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
	  }
	  }
	  }
	  
	  
	  
	  
