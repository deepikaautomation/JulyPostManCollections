pipeline {
    agent any
    stages {

        stage('Checkout') {
            steps {
                echo("Checking out the code...")
                git url: 'https://github.com/naveenanimation20/July2024PostmanCollections'
            }
        }

        stage('Build') {
            steps {
                echo("Building the WAR...")
                // Add actual build steps if needed
            }
        }

        stage('Deploy to QA') {
            steps {
                echo("Deploying to QA...")
                // Add deployment steps if needed
            }
        }

        stage('Run API Test Cases') {
            steps {
                echo("Running API test cases with Newman...")
                bat 'newman run ./SampleAPI.postman_collection.json -e ./SampleAPIDev.postman_environment.json -n 1 -r htmlextra,cli --reporter-htmlextra-export ./results/booking_report.html'
            }
        }

        stage('Publish HTML Extra Report') {
            steps {
                echo("Publishing HTML report...")
                publishHTML([allowMissing: false,
                            alwaysLinkToLastBuild: false, 
                            keepAll: true, 
                            reportDir: 'results', 
                            reportFiles: 'booking_report.html', 
                            reportName: 'HTML Extra API Report', 
                            reportTitles: ''])
            }
        }

        stage('Deploy to PROD') {
            steps {
                echo("Deploying to PROD...")
                // Add actual deployment steps if needed
            }
        }
    }
}
