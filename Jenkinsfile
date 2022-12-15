node{
   stage('STUCRED Checkout'){
     git 'https://github.com/saibuvan1/my-app.git'
   }
   stage('STUCRED-Compile-Package'){

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
   stage('STUCRED Build Docker Imager'){
   sh 'docker build -t saibuvang/myweb:0.0.2 .'
   }
   stage('STUCRED Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u saibuvang -p ${dockerPassword}"
    }
   sh 'docker push saibuvang/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 52.66.15.20:8083"
   sh "docker tag saibuvang/myweb:0.0.2 52.66.15.20:8083/damo:1.0.0"
   sh 'docker push 52.66.15.20:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('STUCRED Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest saibuvang/myweb:0.0.2' 
   }
}
}


