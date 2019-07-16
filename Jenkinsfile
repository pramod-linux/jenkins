pipeline {
    agent any
    parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.122.215', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.122.159', description: 'Production Server')
    }
    triggers {
         pollSCM('* * * * *')
     }
stages{
        stage('Build'){
            steps {
                sh '/usr/local/src/apache-maven/bin/mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
