pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat '''
                echo === BUILD START ===
                if not exist build mkdir build
                echo Sample artifact created on %date% %time% > build\\artifact.txt
                echo === BUILD END ===
                '''
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>✅ Build Successful!</p>
                         <p><b>Job:</b> ${env.JOB_NAME}</p>
                         <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                         <p>🔗 <a href="${env.BUILD_URL}">Open Build in Jenkins</a></p>""",
                to: 'harshvardhanpingane2002@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "❌ FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>❌ Build Failed!</p>
                         <p><b>Job:</b> ${env.JOB_NAME}</p>
                         <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                         <p>🔗 <a href="${env.BUILD_URL}">Check console output</a></p>""",
                to: 'harshvardhanpingane2002@gmail.com'
            )
        }
    }
}
