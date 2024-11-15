pipeline {
    agent any
    tools {
        git 'Default'
    }
    environment {
        MAVEN_HOME = "/opt/maven"  // Path where Maven will be installed
        MAVEN_VERSION = "3.9.9"    // Specify the version of Maven to install
    }
    stages {
        stage('Install Maven') {
            steps {
                script {
                    // Download and install Maven
                    sh """
                        if [ ! -d "${MAVEN_HOME}" ]; then
                            cd /opt
                            wget https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz
                            tar -xvzf apache-maven-${MAVEN_VERSION}-bin.tar.gz
                            mv apache-maven-${MAVEN_VERSION} maven
                            rm apache-maven-${MAVEN_VERSION}-bin.tar.gz
                        fi
                    """
                }
            }
        }
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build WAR') {
            steps {
                // Use the installed Maven to build the WAR file
                sh '${MAVEN_HOME}/bin/mvn clean package'
            }
        }
    }
    post {
        success {
            // Archive the WAR file as an artifact in Jenkins
            archiveArtifacts artifacts: '**/target/*.war', allowEmptyArchive: true
            echo 'WAR file successfully created and archived.'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
