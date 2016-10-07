pipeline {
  environment {
    FOO = "BAR"
  }

  def CMake = fileLoader.fromGit('vars/CMake', 'https://github.com/davidjsherman/aseba-jenkins.git', 'master', null, '')

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
      
      sh 'for d in dashel enki aseba; do (cd $d && git remote -v | sed s/^/$d:/) done'
      
      stash excludes: '.git', name: 'source'
    }
    
    stage("Dashel") {
      unstash 'source'
      CMake.call([buildType: 'Debug',
		  sourceDir: '$workDir/dashel',
		  buildDir: '$workDir/_build/dashel/',
		  installDir: '$workDir/_install',
		  getCmakeArgs: [ '-DBUILD_SHARED_LIBS:BOOL=ON' ]
		 ])
    }
  }
}
