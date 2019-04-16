pipeline {
	agent any
	parameters {
        choice(name: 'NETWORK', choices: ['staging', 'production'], description: 'The network to activate the network list.')
    	string(name: 'CONFIGNAME', defaultValue: 'Manuel'"${env.BUILD_ID}", description: 'The new domain name') 
    }

	stages {
		stage('Messages') {
			steps {
				echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
				//https://gcs.akshayranganath.com/env-vars.html/
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
				//sh "akamai property create ${CONFIGNAME} --clone www.gssclinic.net --hostnames ${CONFIGNAME}.gssclinic.world-tour.akamaideveloper.net --edgehostname gssclinic.world-tour.akamaideveloper.net.edgesuite.net" 
				sh "http -v --auth-type edgegrid -a GSSClinic: :/papi/v1/contracts"
			}
		}
	}
}
