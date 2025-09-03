pipeline {
    agent any
    
    tools {
        maven 'Maven 3.9.11'  // Define Maven tool installed in Jenkins
        jdk 'JDK-17'    // Define JDK installed in Jenkins
    }

    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin123'
        TOMCAT_URL  = 'http://localhost:8080/manager/text'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Harshvardhanpingane/java-login-db-demo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                echo 'Checking Maven and Java'
                bat 'where mvn'
                bat 'where java'
                bat '"D:\\apache-maven-3.9.11\\bin\\mvn.cmd" clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                     // WAR फाईल शोध
                    def warFile = bat(
                        script: 'dir /b target\\*.war',
                        returnStdout: true
                    ).trim()

                    // WAR Tomcat वर deploy करा
                    bat """
                        curl -u %TOMCAT_USER%:%TOMCAT_PASS% ^
                        --upload-file target\\${warFile} ^
                        "%TOMCAT_URL%/deploy?path=/pipelineapp&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deploy Successful!"
        }
        failure {
            echo "❌ Build or Deploy Failed!"
        }
    }
}