pipeline {
    agent { label 'master' }
    triggers {
        cron('* *  ** * *')
    }
    environment{
        MYNAME = 'Saswat'
    }
    stages {
        stage('Stage1') {
            steps {
                git 'https://github.com/Saswat93/simple-java-maven-app.git'
            }
            
        }
        stage('Stage2') {
            agent {label 'dal'}
            steps {
              echo env.MYNAME
            }
        }
        stage ('Post Build') {
            steps {
         build job : 'Deploy'
            }
        }
   }
}
