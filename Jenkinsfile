echo "Changed filename to Cap"

pipeline{
    agent any
    
    stages{
        stage("checkout"){
            steps{
                git branch: 'main', 
                url: 'https://github.com/Speecee/course3-jenkins-gs-spring-petclinic';    
            }
        }
        stage("build"){
            steps{
                powershell "./mvnw package"
            }
        }
        stage("Post build"){
            steps{
                junit allowEmptyResults: true, 
                keepTestNames: true, 
                skipOldReports: true, 
                stdioRetention: 'ALL',
                testResults: '**/target/surefire-reports/*.xml' // This step should not normally be used in your script. Consult the inline help for details.
                archive '**/target/*.jar'
                recordCoverage(tools: [[parser: 'COBERTURA']])
            }
        }
        
    }
    post{
        always{
                emailext body: "${env.BUILD_URL}/${currentBuild.absoluteUrl}",
                to: 'always@foo.bar',
                recipientProviders: [developers()],
                subject: "Job '${JOB_NAME}' (${BUILD_NUMBER})"
        }
       
    }
   
}
