node{
   stage('SCM Checkout'){
     git 'https://github.com/vijay591996/my-app'
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
   sh 'docker build -t vijay050996/myweb:0.0.2 .'
   }
   stage('Docker Image push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u vijay050996 -p ${dockerPassword}"
    }
   sh 'docker push vijay050996/myweb:0.0.2'
   }
   stage('Nexus Image push'){
   sh "docker login -u admin -p 1234 3.16.55.131:8083"
   sh "docker tag vijay050996/myweb:0.0.2 3.16.55.131:8083/vijay:1.0.0"
   sh 'docker push 3.16.55.131:8083/vijay:1.0.0'
   }
	stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest vijay050996/myweb:0.0.2' 
   }
   }
}
