node{
   stage('SCM Checkout'){
     git 'https://github.com/pkgithubpk/my-app.git'
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
   sh 'docker build -t prakash2711/myweb .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u prakash2711 -p ${dockerPassword}"
    }
   sh 'docker push prakash2711/myweb'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 3.80.82.248:8083"
   sh "docker tag prakash2711/myweb 3.80.82.248:8083/prak:1.0"
   sh 'docker push 3.80.82.248:8083/prak:1.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest prakash2711/myweb' 
   }
}
}

