def laboratoryRepositoryUrl = 'https://github.com/mahapajapati/react-games.git'
def laboratoryRepoBranch = "main"
def scriptsFilePath = "${env:JENKINS_HOME}/scripts"


pipeline {
    agent { label 'slave1' }

    parameters {
        booleanParam(name: 'BOOLEAN_PARAM', defaultValue: false, description: 'Pick a value')
        string(name: 'STRING_PARAM', defaultValue: "hello")
        choice(name: 'DROPDOWN_PARAM', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    }

    stages {
        stage('checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "${laboratoryRepoBranch}"]],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [
                      [$class: 'CloneOption', depth: 0, noTags: false, reference: '/home/jenkins/repo', shallow: false, timeout: 120],
                      [$class: 'PruneStaleBranch'],
                      [$class: 'CheckoutOption', timeout: 120]
                    ],
                    userRemoteConfigs: [[url: "${laboratoryRepositoryUrl}"]]
                ])
            }
        }
        stage('build'){
            steps{
                script{
                    echo "The STRING_PARAM is: ${params.STRING_PARAM}"
                    sh "/home/jenkins/scripts/build_script.sh ${params.STRING_PARAM}"
                }
            }
        }

        stage ('test') {
            steps {
                script {
                    echo "The BOOLEAN_PARAM is: ${params.BOOLEAN_PARAM}"
                    if (params.BOOLEAN_PARAM) {
                        echo "You have chosen ${params.BOOLEAN_PARAM}"
                    } else {
                        echo "You have chosen ${params.BOOLEAN_PARAM}"
                    }
                }
            }
        }

        stage ('deploy') {
            steps {
                script {
                    echo "You have chosen ${params.DROPDOWN_PARAM}"
                }
            }
        }
    }

    post {
        always {
            echo 'Post: Archive Artifacts'
            archiveArtifacts artifacts: '/home/jenkins/scripts/*.txt', onlyIfSuccessful: true
        }
    }
}
