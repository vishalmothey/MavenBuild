node('master'){
	// tools {
	// 	maven 'maven360'
	// }

	
	stage ('checkout code'){
		checkout scm
	}
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}
	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}
	
	stage ('Sonar Analysis'){
		def mvn = tool 'maven363';
    		withSonarQubeEnv() {
      		sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=test_project -Dsonar.projectName='test_project'"
    		}
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}

	stage ('Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://13.127.175.94:8080/')], contextPath: 'counterwebapp', war: 'target/*.war'
	}

	stage ('Notification'){
		emailext (
			subject: "Job Completed",
			body: "Jenkins Pipeline Job for Maven Build got completed !!!",
			to: "build-alert@example.com"
		)
	}
}
