node('pearlSlaveNode') {

    sh 'java -version'
    sh 'mvn -version'

    
    def buildInfo
	

            
	stage 'Checkout'
    	when branch 'DEV'

		checkout([$class: 'GitSCM', branches: [[name: '*/master']],   				doGenerateSubmoduleConfigurations: false, 
			extensions: [[$class: 'CleanBeforeCheckout']], submoduleCfg: [], 
			userRemoteConfigs: [[credentialsId: 'jenkins-pearl-github', 
			url: 'https://github.cicd.pearl.local/pearl/pricing-deployer.git']]])
	
	stage 'Artifactory configuration'
	
	stage 'Build'

	stage 'Push Artifactory'


	sh '''#!/bin/sh
    
	role_to_assume_arn="arn:aws:iam::704055408335:role/ProdJenkinsProj50CrossAccount"
    
	# Get crednetials

	assumeJson=`aws sts assume-role --role-arn \${role_to_assume_arn} --role-session-name 		JenkinsAssume`
 
	secretKey=`echo $assumeJson  | jq -r .Credentials.SecretAccessKey`
	accessKey=`echo $assumeJson | jq -r .Credentials.AccessKeyId`
	token=`echo  $assumeJson | jq -r .Credentials.SessionToken`
	# Set as environment variables
	export AWS_ACCESS_KEY_ID=$accessKey
	export AWS_SECRET_ACCESS_KEY=$secretKey
	export AWS_SESSION_TOKEN=$token
	export AWS_DEFAULT_REGION=us-east-1

	'''
}

