pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'sushu_id', url: 'https://github.com/github-33/spring3-mvc-maven-xml-hello-world.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }


        }
        stage("Artifact"){
            steps{
                archiveArtifacts artifacts: 'target/*.war'
            }
        }
         stage("deploy"){
            steps{
                withCredentials([usernameColonPassword(credentialsId: 'tomcat_server', variable: 'tomcat_credentials')]) 
                {
                sh "curl -u $tomcat_credentials -T /var/lib/jenkins/workspace/maven_pipeline/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war 'http://ec2-3-133-102-44.us-east-2.compute.amazonaws.com:8080/manager/text/deploy?path=/jenkins_Deploy&update=true'"
                }

            }
        }
        }
    }

