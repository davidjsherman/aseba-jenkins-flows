#groovy

pipeline {
    environment {
        FOO = "BAR"
    }

    agent label:'inirobot-deb64'

    stages {
        stage("Setup") {
            sh 'mkdir -p dashel enki aseba'

	    dir('dashel') {
	      git branch: 'master', url: 'https://github.com/aseba-community/dashel.git'
	    }
	    dir('enki') {
	      git branch: 'master', url: 'https://github.com/enki-community/enki.git'
	    }
	    dir('aseba') {
	      git branch: 'master', url: 'https://github.com/aseba-community/aseba.git'
	      sh 'git submodule update --init --recursive'
	    }
        }
    }
}
