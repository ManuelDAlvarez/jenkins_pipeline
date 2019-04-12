pipeline {
	agent any
	stages {
		stage('InitialMessage') {
			steps {
				echo 'Runing build stage'
			}
		}
		stage('apsec') {
			steps {
				sh 'akamai install apsec'
			}
		}
		stage('cps') {
			steps {
				sh 'akamai install cps'
			}
		}
		stage('firewall') {
			steps {
				sh 'akamai install firewall'
			}
		}
	}
}
