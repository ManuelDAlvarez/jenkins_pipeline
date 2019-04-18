pipeline {
	agent any
	parameters {
        choice(name: 'NETWORK', choices: ['staging', 'production'], description: 'The network to activate the network list.')
    	string(name: 'CONFIGNAME', defaultValue: "${env.BUILD_ID}", description: 'The new domain name') 
    }
    environment {
    	PROJ = "/bin:/usr/local/bin:/usr/bin"
	}
	stages {
		stage('Messages') {
			steps {
				echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
				echo "You say ${NETWORK} for ${CONFIGNAME}"
				//sh "http --auth-type edgegrid -a default: GET :/papi/v1/contracts | jq "
			}
		}
		stage('cloneconfig') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					//sh "akamai property create ${CONFIGNAME} --clone www.gssclinic.net --hostnames ${CONFIGNAME}.gssclinic.world-tour.akamaideveloper.net --edgehostname gssclinic.world-tour.akamaideveloper.net.edgesuite.net" 
					sh "akamai property create ${CONFIGNAME} --clone PROD.bigmanuel --hostnames ${CONFIGNAME} --edgehostname manueltest2.edgesuite.net"
					//sh "http --auth-type edgegrid -a GSSClinic: GET :/papi/v1/contracts | jq "
				}
			}
		}
		stage('updateJSON') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					sh "akamai property retrieve ${CONFIGNAME} --file metadata.json"
					//sh "akamai property retrieve PROD.bigmanuel --file metadata.json"
					archiveArtifacts "metadata.json"
					sh "cat metadata.json"
					sh "sed 's/378312/371349/g' metadata.json > 2metadata.json"
					archiveArtifacts "metadata.json"
					archiveArtifacts "2metadata.json"
					//sh "sed -i -n s/810121/842943/g metadata.json"
					sh "cat metadata.json"
					sh "cat 2metadata.json"
				}
			}
		}
		stage('updateConfiguration') {https://open.spotify.com/album/6BkeUWI72Lssc077AxqQek
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					sh "akamai property update ${CONFIGNAME} --file 2metadata.json"
					//echo "yes"
				}
			}
		}
		stage('activatedelivery') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					sh "akamai property activate ${CONFIGNAME} --network ${NETWORK}"
					//echo "yes too"
				}
			}
		}
	}
}
