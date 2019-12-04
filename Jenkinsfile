node {
    
    jdk = tool name: 'jenkins-jdk'
    env.JAVA_HOME = "${jdk}"
    
	stage('GIT CHECKOUT') {    
		git url: 'https://github.com/rathoremayank/devops-mindtree-20191115.git'
	}

	def project_path = "atmosphere/spring-boot-samples/spring-boot-sample-atmosphere"
	dir(project_path) {

		stage('MAVEN CLEAN') {
			sh label: 'Clean', script: 'mvn clean'
		}
		stage('MAVEN COMPILATION') {
			sh label: 'Comile', script: 'mvn compile'
		}
		stage('MAVEN TEST') {
			sh label: 'Testing', script: 'mvn test'
		}

		stage('MAVEN PACKAGE') {
			sh label: 'Testing', script: 'mvn package'
		}

		stage('ARCHIVING PACKAGE') {
			archive 'target/*.jar'
		}

		stage('CREATING DOCKER IMAGE') {
			sh label: 'whoami', script: 'whoami'
			sh label: 'Docker', script: 'docker-compose up -d --build'
		}
		   
		stage('PREPARING ANSIBLE') {
			sh label: 'Docker', script: 'cp -rf target/*.jar ../../../terraform/ansible/templates/atmosphere-v1.jar'
			sh label: 'Jenkins', script: "echo '<h1> TASK BUILD ID: ${env.BUILD_DISPLAY_NAME}</h1>' > ../../../terraform/ansible/templates/jenkins.html"
		}   
			
		stage('EXECUTING TERRAFROM SCRIPTS') {
			def project_path_1 = "../../../terraform"
			dir(project_path_1) {    
			sh label: 'terraform', script: '/bin/terraform  init'
			sh label: 'terraform', script: '/bin/terraform  plan'
			sh label: 'terraform', script: '/bin/terraform  apply -input=false -auto-approve'
			}   
		}   
		
	}    
		
}
