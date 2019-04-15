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
				sh "akamai ${env.BUILD_ID}"
			}
		}
		stage('echos') {
			steps {
				echo "You say ${NETWORK} for ${CONFIGNAME}"
				sh "pwd"
			}
		}
		stage('thaos') {
			steps {
				sh "http --auth-type edgegrid -a default: :/papi/v1/contracts/" 
			}
		}
	}
}
