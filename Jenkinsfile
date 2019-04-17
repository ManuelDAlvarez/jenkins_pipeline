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
				echo "You say ${NETWORK} for ${CONFIGNAME}"
			}
		}
		stage('cloneconfig') {
			steps {
				//sh "akamai property create ${CONFIGNAME} --clone www.gssclinic.net --hostnames ${CONFIGNAME}.gssclinic.world-tour.akamaideveloper.net --edgehostname gssclinic.world-tour.akamaideveloper.net.edgesuite.net" 
				sh "akamai property create ${CONFIGNAME} --clone bigmanuel.edgesuite.net --hostnames ${CONFIGNAME} --edgehostname bigmanuel.edgesuite.net"
				//sh "http --auth-type edgegrid -a GSSClinic: GET :/papi/v1/contracts | jq "
			}
		}
		stage('getconfig') {
			steps {
				sh "akamai property retrieve ${CONFIGNAME} > metadata.json"
			}
		}
		stage('updatecpcode') {
			steps {
				sh "sed -i -n 's/378312/371349/g' metadata.json"
				//sh "sed -i -n s/810121/842943/g metadata.json"	
			}
		}
		stage('updateconfiguration') {
			steps {
				sh "akamai property update ${CONFIGNAME} --file metadata.json"
			}
		}
		stage('activatedelivery') {
			steps {
				sh "akamai property activate ${CONFIGNAME} --network ${NETWORK}"
			}
		}
	}
}
