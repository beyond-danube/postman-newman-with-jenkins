pipeline {
    agent any 
    tools { nodejs 'Node-18.x' }
    environment {
        API_KEY = credentials('postman-newman-examples-key')
    }
    parameters {
        string(name: 'COLLECTION_ID', defaultValue: '', description: 'Postman collection, UUID')
    }
    stages {
        stage('Get Collection') { 
            steps {
                sh 'curl --location https://api.postman.com/collections/$COLLECTION_ID?access_key=$API_KEY -o coll.json'
            }
        }
        stage('Run Collection') {
            steps {
                sh 'newman run coll.json -r htmlextra,cli,junitfull --reporter-htmlextra-export index.html --reporter-junitfull-export result.xml'

            }
        }
    }
    
    post {
        always {
            junit '**/result.xml'
            publishHTML (target: [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '',
                    reportFiles: 'index.html',
                    reportName: "Newman Report"
            ])
            
            cleanWs()
        }
    }
}
