pipeline {
    agent any

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Choose target environment')
    }

    triggers {
        // Poll SCM optional, can comment out if relying on webhook
        // pollSCM('H/2 * * * *') 
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Harshvardhanpingane/java-login-db-demo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                bat '''
                echo === BUILD START ===
                echo Date: %date% %time%
                if not exist build mkdir build
                echo Sample artifact 1 created by Jenkins on %date% %time% > build\\artifact1.txt
                echo Sample artifact 2 created by Jenkins on %date% %time% > build\\artifact2.txt
                echo === BUILD END ===
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive all txt files in build folder
                archiveArtifacts artifacts: 'build\\*.txt', onlyIfSuccessful: true
            }
        }

        stage('Deploy') {
            when { expression { params.ENV in ['dev','qa','prod'] } }
            steps {
                bat """
                set TARGET_DIR=C:\\deploy\\%ENV%
                if not exist %TARGET_DIR% mkdir %TARGET_DIR%
                copy /Y build\\*.txt %TARGET_DIR%\\
                echo Deployed artifacts to %TARGET_DIR%
                """
            }
        }
    }

    post {
        success {
            echo "✅ ${env.JOB_NAME} #${env.BUILD_NUMBER} finished OK (ENV=${params.ENV})"
        }
        failure {
            // Email on failure
            emailext(
                subject: "❌ Jenkins FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                to: 'harshvardhanpingane2002@gmail.com',
                body: """Build failed.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
ENV: ${params.ENV}
Console: ${env.BUILD_URL}console
"""
            )
        }
    }
}
