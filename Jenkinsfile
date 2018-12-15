pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
              echo 'Now Building...'
            // Unix
                sh '/Users/ivy.shih/tools/apache-maven-3.6.0/bin/mvn clean package'
            // Winblows
            //    bat '"C:\\Program Files\\Tools\\apache-maven-3.5.4\\bin\\mvn.cmd" clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                build job: 'deploy2stage'
            }
        }

        stage ("Deploy to Production"){
              steps {
               //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                build job: 'deploy2prod'
              }
              post {
                  success {
                    echo 'Code deployed to Production.'
                  }
                  failure {
                    echo 'Deployment failed'
                  }
              }
        }
 }

}
