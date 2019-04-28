pipeline {
	agent any
	parameters {
        choice(name: 'NETWORK', choices: ['staging', 'production'], description: 'The network to activate the configuration.')
    	string(name: 'CONFIGNAME', defaultValue: "${env.BUILD_ID}", description: 'The domain you are adding to the configurations') 
    	//string(name: 'CONFIGNAME', defaultValue: "manueltest33.edgesuite.net", description: 'The new domain name') 
    }
    environment {
    	PROJ = "/bin:/usr/local/bin:/usr/bin"
	}
	stages {
		stage('Messages') {
			steps {
				//echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
				echo "You say ${NETWORK} for ${CONFIGNAME}"
				//sh "http --auth-type edgegrid -a default: GET :/papi/v1/contracts | jq "
				slackSend(botUser: true, message: "${env.JOB_NAME} will create ${CONFIGNAME} add it to the DPP and push it to ${NETWORK} for ", color: '#1E90FF')
			}
		}
		stage('cloneconfig') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					//sh "akamai property create ${CONFIGNAME} --clone www.gssclinic.net --hostnames ${CONFIGNAME}.gssclinic.world-tour.akamaideveloper.net --edgehostname gssclinic.world-tour.akamaideveloper.net.edgesuite.net" 
					//sh "akamai property create ${CONFIGNAME} --clone PROD.bigmanuel --hostnames ${CONFIGNAME} --edgehostname manueltest2.edgesuite.net"
					sh "akamai property create ${CONFIGNAME} --clone manuel39.edgesuite.net --hostnames ${CONFIGNAME} --edgehostname manuel.origin.edgekey.net"
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
					sh "sed 's/371349/378312/g' metadata.json > 2metadata.json"
					//archiveArtifacts "metadata.json"
					archiveArtifacts "2metadata.json"
					//sh "sed -i -n s/810121/842943/g metadata.json"
					sh "cat metadata.json"
					sh "cat 2metadata.json"
				}
			}
		}
		stage('updateConfiguration') {
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
		stage('CreateNewVersion') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					//sh "akamai appsec configs --section default --json | jq '.configurations[] | select (.name | test (\"gss-ta-demo\"))'"
					sh "akamai appsec --section default clone --config=40539 --version=STAGING"
					//${NETWORK}"
				}
			}
		}
		stage('AddHostname') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					sh "akamai appsec --section default add-hostname --config=40539 ${CONFIGNAME}"
				}
			}
		}
		stage('ClonePolicy') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					script {
                 	   def policyId = sh(script: "akamai appsec --section default clone-policy ODPP_78900 --config=40539  --prefix=${env.BUILD_ID}  --json | jq '.policyId'", returnStdout: true).trim()
                 	   println("policyId = ${policyId}")
                 	   sh(script: "akamai appsec --section default create-match-target --config=40539 --hostnames=${CONFIGNAME} --paths='/*' --policy=${policyId} ", returnStdout: true).trim()
                 	   
                	}
				}
			}
		}
		stage('ActivateConfig') {
			steps {
				withEnv(["PATH+EXTRA=$PROJ"]) {
					sh "akamai appsec activate --section default --config=40539 --network=${NETWORK} --notes='automated' --notify=maalvare@akamai.com"
				}
			}
		}

	}
}
