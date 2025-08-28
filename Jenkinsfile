pipeline {
  agent any

  options {
    //timestamps()
    buildDiscarder(logRotator(numToKeepStr: '15'))
  }

  parameters {
    choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Choose target environment')
  }

  triggers {
    // Optional: webhook नसेल तर
    pollSCM('H/2 * * * *') // ~ दर 2 मिनिटांनी बदल तपासा
  }

  stages {
    stage('Checkout') {
      steps {
        // PUBLIC repo:
          git branch: 'main', url: 'https://github.com/Harshvardhanpingane/java-login-db-demo.git'
      }
    }

    stage('Build') {
      steps {
        bat '''
        echo === BUILD START ===
        echo Date: %date% %time%
        if not exist build mkdir build
        echo Sample artifact created by Jenkins on %date% %time% > build\\artifact.txt
        echo === BUILD END ===
        '''
      }
    }

    stage('Archive Artifact') {
      steps {
        archiveArtifacts artifacts: 'build\\artifact.txt', onlyIfSuccessful: true
      }
    }

    stage('Deploy') {
      when { expression { params.ENV in ['dev','qa','prod'] } }
      steps {
        bat """
        set TARGET_DIR=C:\\deploy\\%ENV%
        if not exist %TARGET_DIR% mkdir %TARGET_DIR%
        copy /Y build\\artifact.txt %TARGET_DIR%\\artifact-%ENV%.txt
        echo Deployed artifact to %TARGET_DIR%
        """
      }
    }
  }

  post {
    success {
      echo "✅ ${env.JOB_NAME} #${env.BUILD_NUMBER} finished OK (ENV=${params.ENV})"
    }
    failure {
      emailext(
        subject: "❌ Jenkins FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        to: 'your_email@example.com',
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
