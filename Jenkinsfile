node{
   stage('SCM Checkout'){
     git 'https://github.com/msrishakthi02/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t srishakthi/myweb:0.0.2 .'
   }
   stage('Docker Image Push to container'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u srishakthi -p ${dockerPassword}"
    }
   sh 'docker push srishakthi/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
//    sh "docker login -u srishakthi -p Akilandeshwari@27 3.110.223.182:8083"
//    sh "docker tag srishakthi/myweb:0.0.3 3.110.223.182:8083/srishakthi:1.0.0"
//    sh 'docker push 3.110.223.182:8083/srishakthi:1.0.0'
  }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest srishakthi/myweb:0.0.2' 
   }
}
}



