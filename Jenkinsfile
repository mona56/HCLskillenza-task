pipeline{
    agent {label 'master'}
    
    tools {
        maven "M3maven"
    }
    
    stages {
        
        stage ("GIT CHECKOUT"){
            steps {
              
                git 'https://github.com/mona56/HCLskillenza-task.git'
                echo "=================== My clone Completed==================="
            }
        }
        
        stage ('Perform Sonar Analysis'){
            steps {
                echo "========Sonar Analysis Started========="
                sh '''
                "E:\\sonar-scanner-cli-4.2.0.1873-windows\\sonar-scanner-4.2.0.1873-windows\\bin\\sonar-scanner.bat"
                '''
            }
        }
		 stage("SonarQube Quality Gate") { 
        timeout(time: 1, unit: 'HOURS') { 
           def qg = waitForQualityGate() 
           if (qg.status != 'OK') {
             error "Pipeline aborted due to quality gate failure: ${qg.status}"
           }
        }
    }
	stage('approve') {
    timeout(time: 1, unit: 'MINUTES') {
        input message: 'Do you want to continue?'
    }
}
        
        stage ("Build Code"){
            steps{
                sh "mvn clean package"
                  
            }
        }
        
        stage ("Upload Artifacts"){
            steps{
                script{
            def server = Artifactory.server 'jfrog'        
            def uploadSpec = """{   
            "files": [{
            
            "pattern": "*",
            "target": "DAL-pipeline/",
            "recursive": "false"
        }]
            }"""
            server.upload(uploadSpec)
            
      }
     }
	 }
	 
	 stage ("Publish Junit Report"){
	     steps{
	         junit '**/target/surefire-reports/TEST-*.xml'
	         archiveArtifacts '*'
	     }
	 }
stage("Ansible deployment") {
        sh '''
        echo "Executing playbook"
        
        ansible-playbook --syntax-check ./tomcat.yml
        ''' }
}
}
