pipeline {
	agent any
	parameters {
        choice(name: 'NETWORK', choices: ['staging', 'production'], description: 'The network to activate the network list.')
    	string(name: 'CONFIGNAME', defaultValue: "${env.BUILD_ID}", description: 'The new domain name') 
    }

	stages {
		stage('Messages') {
			steps {
				echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
			}
		}
		stage('echos') {
			steps {
				echo "You say ${NETWORK} for ${CONFIGNAME}"
				sh "jq"
			}
		}
		stage('cloneconfig') {
			steps {
				//sh "akamai property create maalvare-${env.BUILD_ID} --clone www.gssclinic.net --hostnames ${env.BUILD_ID}.gssclinic.world-tour.akamaideveloper.net --edgehostname gssclinic.world-tour.akamaideveloper.net.edgesuite.net" 
				sh "http --auth-type edgegrid -a GSSClinic: :/papi/v1/contracts"
			}
		}
	}
}
