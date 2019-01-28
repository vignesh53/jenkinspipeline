pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.222.231.172', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.15.202.239', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh '/Users/vigneshd/Documents/apache-maven-3.6.0/bin/mvn clean package'
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
                        sh "scp -i /Users/vigneshd/Documents/JenkinsDemo/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/vigneshd/Documents/JenkinsDemo/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}