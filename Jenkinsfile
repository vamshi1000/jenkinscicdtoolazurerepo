import groovy.json.JsonSlurper

def getFtpPublishProfile(def publishProfilesJson) {
  def pubProfiles = new JsonSlurper().parseText(publishProfilesJson)
  for (p in pubProfiles)
    if (p['publishMethod'] == 'FTP')
      return [url: p.publishUrl, username: p.userName, password: p.userPWD]
}

node {
  withEnv(['AZURE_SUBSCRIPTION_ID=e44c9379-2fc4-4ec0-8945-b7fd3ff798bd',
        'AZURE_TENANT_ID=0c508b0d-c4b2-4a38-93c7-5947fb5a5656']) {
    stage('init') {
      checkout scm
    }
  
    stage('build') {
      sh 'mvn package'
    }
  
    stage('deploy') {
	
	    withCredentials([usernamePassword(credentialsId: '45017c78-9445-417b-9c33-558022133be8', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')])
		{
        sh 'mvn package azure-webapp:deploy  -Dazure.client=${USERNAME_VAR} -Dazure.key=${PASSWORD_VAR}'
        }
    }
  }
}
