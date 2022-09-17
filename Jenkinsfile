import groovy.json.JsonSlurper

def getFtpPublishProfile(def publishProfilesJson) {
  def pubProfiles = new JsonSlurper().parseText(publishProfilesJson)
  for (p in pubProfiles)
    if (p['publishMethod'] == 'FTP')
      return [url: p.publishUrl, username: p.userName, password: p.userPWD]
}

node {
  withEnv(['AZURE_SUBSCRIPTION_ID=6003c517-c410-4a40-b9c5-2e288251125e',
        'AZURE_TENANT_ID=76a2ae5a-9f00-4f6b-95ed-5d33d77c4d61']) {
    stage('init') {
      checkout scm
    }
  
    stage('build') {
      sh 'mvn package'
    }
  
    stage('deploy') {
	
	    withCredentials([usernamePassword(credentialsId: '45017c78-9445-417b-9c33-558022133be8', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')])
		{
			sh ' az login --service-principal -u $USERNAME_VAR -p $PASSWORD_VAR -t $AZURE_TENANT_ID'
        sh 'mvn package azure-webapp:deploy  -Dazure.client=${USERNAME_VAR} -Dazure.key=${PASSWORD_VAR}'
        }
    }
  }
}
