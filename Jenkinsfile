pipeline {
    agent any

    tools {

        maven 'maven 3.0.3'
	jdk 'localJDK'

	}

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.89.165.41', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.163.21.239', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "scp -i /c/Users/Sanand/Downloads/MyNVKeypair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /c/Users/Sanand/Downloads/MyNVKeypair.pem  **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
