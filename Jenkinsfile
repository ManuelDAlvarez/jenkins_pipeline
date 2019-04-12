pipeline {
	agent any
	stages {
		stage('InitialMessage') {
			steps {
				echo 'Runing build stage'
			}
		}
		stage('listConfig') {
			steps {
				sh 'akamai list --remote'
			}
		}
	}
}
