// Patrick Nguyen A10300965 April 14, 2020 - 

pipeline {
    agent any
    stages {
        stage("Build") {
            steps {
                echo "Installing the requirements"
                sh "pip install -r requirements.txt"
                echo "Done requirements"
            }
        }
        stage("Code Quality") {
            steps {
                script {
                    def files = findFiles glob: "*.py"
                    for (int i = 0; i < files.size(); ++i) {
                        sh "pylint-fail-under --fail_under 3 ${files[i]}" 
                    }
                }
            }
        }
        stage("Code Quantity") {
            steps {
                sh "wc -l *.py "
            }
        }
        // stage("Run") {

        // }
        stage("Package") {
            steps {
                sh "zip package.zip *.py"
                archiveArtifacts artifacts: "package.zip", fingerprint: true
            }
        }
    }
}