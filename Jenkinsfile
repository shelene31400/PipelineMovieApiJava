pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "JDK11"
    }

    stages {
        stage('Clean/Compile') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'master' ,
                    url: 'https://github.com/matthcol/movieapijava2021'

                // Run Maven on a Unix agent.
                sh "mvn clean compile"
            }
        }
		stage('Test') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'dev' ,
                    url: 'https://github.com/matthcol/movieapijava2021'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true test"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
		        stage('Package') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'dev' ,
                    url: 'https://github.com/matthcol/movieapijava2021'

                // Run Maven on a Unix agent.
                sh "mvn -DskipTests -Dmaven.test.failure.ignore=false package"
            }
        }
    }
}