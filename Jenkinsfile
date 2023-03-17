pipeline {
    agent any
    
    triggers { pollSCM('* * * * *') }
     
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', credentialsId: '61f055ec-da2a-4c63-9d24-f0991dedf02d', url: 'https://github.com/statinbower/jgsu-spring-petclinic.git'

                // // Run Maven on a Unix agent.  
                // sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // // To run Maven on a Windows agent, use
                // // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('package') {
            steps {
                // // // Get some code from a GitHub repository
                // // git 'https://github.com/jglick/simple-maven-project-with-tests.git'

                // // Run Maven on a Unix agent.
                sh './mvnw clean package'

                // // To run Maven on a Windows agent, use
                // // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                // }
                // changed{
                    emailext attachLog: true, body: 'body: "Please go to ${BUILD_URL} and verify the build"',
                    compressLog: true, recipientProviders: [upstreamDevelopers(), requestor()], 
                    subject: 'Job \'${JOB_NAME}\' (${BUILD_NUMBER}) is successful', to: 'kishorekumar3015@outlook.com'

                }
            }
        }
    }
}
