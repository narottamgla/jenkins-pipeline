import groovy.json.JsonOutput
import groovy.json.JsonSlurper


pipeline {
    agent any

    stages {
        stage ('Compile Stage') {
            
          steps {
            git branch: 'master',
                credentialsId: 'Github_Cred',
                url: 'https://github.com/narottamgla/selenium-bdd-cucumber.git'
        }
        }

        stage ('Testing Stage:Firefox') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                  //  bat 'mvn verify -Dcontext=firefox -Dwebdriver.driver=firefox'
                   getCurrentHerokuReleaseDate()
                   echo 'Firefox...'

                }
            }
        }
        
         stage ('Testing Stage: Chrome') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                  // bat 'mvn verify -Dcontext=chrome -Dwebdriver.driver=chrome'
                 echo 'Chome...'

                }
            }
        }


        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_5_0') {
                    echo 'Deployment...'
                }
            }
        }
        
        stage ('Publish Reports') {
            steps {
              // publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: '\\target\\site\\serenity', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
           echo "Report"
            }
        }
    }
}


def getCurrentHerokuReleaseDate() {
   // withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
      //  def apiUrl = "https://api.heroku.com/apps/${app}/releases/${version}"
        def response = sh(returnStdout: true, script: "curl --location --request GET 'https://api.heroku.com/apps' \
--header 'Accept: application/vnd.heroku+json; version=3' \
--header 'Authorization: Basic bmFyb3R0YW1nbGFAZ21haWwuY29tOlNpbmdoMTk5MyM='").trim()
        def jsonSlurper = new JsonSlurper()
        response = "{\"created_at\": \"2020-01-23T16:51:26Z\",\r\n        \"id\": \"119ac296-8d51-4f78-8047-0ab8b8419146\",\r\n        \"maintenance\": false\r\n}"
        def data = jsonSlurper.parseText("${response}")
        return data.created_at
   // }
}
