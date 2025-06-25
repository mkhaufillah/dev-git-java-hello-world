pipeline {
    agent any

    environment {
        TEST_RESULT = '' // Global variable to store test result
    }

    stages {
        stage('Build') {
            steps {
                echo 'Cloning and compiling HelloWorld.java...'
                sh '''
                    rm -rf dev-git-java-hello-world
                    git clone https://github.com/mkhaufillah/dev-git-java-hello-world
                    cd dev-git-java-hello-world
                    git checkout main
                    javac HelloWorld.java
                    cd ..
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Cloning and running tests from bootcamp-batch3...'
                script {
                    sh '''
                        rm -rf bootcamp-batch3
                        git clone https://github.com/afteroffice466/bootcamp-batch3
                        cd bootcamp-batch3
                        git checkout headless-chrome
                        mvn install -DskipTests
                    '''

                    // Capture mvn test output
                    def result = sh(
                        script: 'cd bootcamp-batch3 && mvn test -DsuiteXml=src/test/resources/selenium_page_factory/runner_regresion_cucumber.xml -Denv=production',
                        returnStdout: true
                    ).trim()

                    env.TEST_RESULT = result
                    echo "Test Result:\n${env.TEST_RESULT}"
                }
            }
        }

        stage('Deployment') {
            steps {
                echo 'Running HelloWorld Java application...'
                sh '''
                    cd dev-git-java-hello-world
                    java HelloWorld
                '''
            }
        }
    }

    post {
        always {
            echo "Final test result (saved variable):\n${env.TEST_RESULT}"
        }
    }
}
