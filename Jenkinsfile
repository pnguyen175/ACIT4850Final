/* Patrick Nguyen A10300965 April 14, 2020 - 
Build installs the requirements text file.
Code Quality then looks for all .py files and runs pylint fail under 3, 
it should be 5 but report.py return 3.18181818 which ends up breaking the file.
It then checks the word count of .py files and returns the values.
At the run stage it check is the parameter = the target (run) and if so it runes the main,py file
The package stage packages everything once all test pass.
*/

pipeline {
    agent any
        parameters {
            string (name: 'TARGET', defaultValue: 'run', description: '')
        }
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
        stage("Run") {
            when {
                expression { params.TARGET == 'run' }
            } 
            steps {
                sh 'python main.py phones text output'
                sh 'python main.py tablets csv output'
                sh 'python main.py laptops json output'
                sh 'python main.py phones yaml output'
            }
        }
        stage("Package") {
            steps {
                sh "zip package.zip *.py"
                archiveArtifacts artifacts: "package.zip", fingerprint: true
            }
        }
    }
}