pipeline {
	agent any
	stages {
		stage('InitialMessage') {
			steps {
				echo 'Runing build stage'
			}
		}
		stage('echos') {
			steps {
				echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
			}
		}
		stage('cps') {
			steps {
				sh "akamai ${env.BUILD_ID}"
			}
		}
	}
}
