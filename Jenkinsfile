pipeline {
    agent any

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
                sh '''
                    rm -rf bootcamp-batch3
                    git clone https://github.com/afteroffice466/bootcamp-batch3
                    cd bootcamp-batch3
                    git checkout headless-chrome
                    mvn install -DskipTests
                    mvn test -DsuiteXml=src/test/resources/selenium_page_factory/runner_regresion_cucumber.xml -Denv=production
                    cd ..
                '''
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
            echo 'Pipeline selesai.'
        }
    }
}
