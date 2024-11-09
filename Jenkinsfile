pipeline {
    agent any
    environment {
        MAVEN_HOME = "/opt/maven"  // The path where Maven will be installed
        MAVEN_VERSION = "3.8.7"    // Specify the version of Maven to install
    }
    stages {
        stage('Install Maven') {
            steps {
                script {
                    // Check if Maven is already installed
                    if (!fileExists("${MAVEN_HOME}/bin/mvn")) {
                        echo "Maven not found. Installing Maven..."

                        // Download and install Maven
                        sh """
                            cd /opt
                            wget https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz
                            tar -xvzf apache-maven-${MAVEN_VERSION}-bin.tar.gz
                            mv apache-maven-${MAVEN_VERSION} maven
                            rm apache-maven-${MAVEN_VERSION}-bin.tar.gz
                        """
                    } else {
                        echo "Maven already installed."
                    }
                }
            }
        }
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository
                git branch: 'main', url: 'https://github.com/S9787/mynewrep.git'
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
