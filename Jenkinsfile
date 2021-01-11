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
                    bat 'mvn verify -Dcontext=firefox -Dwebdriver.driver=firefox'
                }
            }
        }
        
         stage ('Testing Stage: Chrome') {

            steps {
                withMaven(maven : 'maven_3_5_0') {
                    bat 'mvn verify -Dcontext=chrome -Dwebdriver.driver=chrome'
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
               publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: '\\target\\site\\serenity', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
            }
        }
    }
}


def getCurrentHerokuReleaseDate(app, version) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
        def apiUrl = "https://api.heroku.com/apps/${app}/releases/${version}"
        def response = sh(returnStdout: true, script: "curl -s  -H \"Authorization: Bearer ${env.HEROKU_API_KEY}\" -H \"Accept: application/vnd.heroku+json; version=3\" -X GET ${apiUrl}").trim()
        def jsonSlurper = new JsonSlurper()
        def data = jsonSlurper.parseText("${response}")
        return data.created_at
    }
}
