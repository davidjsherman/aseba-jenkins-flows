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
      // dir('enki') {
      // 	git branch: 'master', url: 'https://github.com/enki-community/enki.git'
      // }
      // dir('aseba') {
      // 	git branch: 'master', url: 'https://github.com/aseba-community/aseba.git'
      // 	sh 'git submodule update --init --recursive'
      // }
      
      // sh 'for d in dashel enki aseba; do (cd $d && git remote -v | sed s/^/$d:/) done'
      
      // stash excludes: '.git', name: 'source'

    }
    
    stage("Dashel") {
      parallel (
	"debian": {
	  asebaCMakeComponent([ component: 'dashel', label: 'inirobot-deb64' ])
	}
      )
    }
      // parallel (
      // 	"ubuntu": {
      // 	  node('inirobot-u64') {
      // 	    unstash 'source'
      // 	    CMake([buildType: 'Debug',
      // 		   sourceDir: '$workDir/dashel',
      // 		   buildDir: '$workDir/_build/dashel/',
      // 		   installDir: '$workDir/_install',
      // 		   getCmakeArgs: [ '-DBUILD_SHARED_LIBS:BOOL=ON' ]
      // 		  ])
      // 	    script {
      // 	      env.dashel_DIR = sh ( script: 'dirname $(find _install -name dashelConfig.cmake | head -1)', returnStdout: true).trim()
      // 	    }
      // 	  }
      // 	},
      // 	"macOS": {
      // 	  node('inirobot-osx') {
      // 	    unstash 'source'
      // 	    CMake([buildType: 'Debug',
      // 		   sourceDir: '$workDir/dashel',
      // 		   buildDir: '$workDir/_build/dashel/',
      // 		   installDir: '$workDir/_install',
      // 		   getCmakeArgs: [ '-DBUILD_SHARED_LIBS:BOOL=ON' ]
      // 		  ])
      // 	    script {
      // 	      env.dashel_DIR = sh ( script: 'dirname $(find _install -name dashelConfig.cmake | head -1)', returnStdout: true).trim()
      // 	    }
      // 	  }
      // 	},
      // 	"windows": {
      // 	  node('inirobot-win7') {
      // 	    unstash 'source'
      // 	    CMake([buildType: 'Debug',
      // 		   sourceDir: '$workDir/dashel',
      // 		   buildDir: '$workDir/_build/dashel/',
      // 		   installDir: '$workDir/_install',
      // 		   getCmakeArgs: [ '-DBUILD_SHARED_LIBS:BOOL=ON' ]
      // 		  ])
      // 	    script {
      // 	      env.dashel_DIR = sh ( script: 'dirname $(find _install -name dashelConfig.cmake | head -1)', returnStdout: true).trim()
      // 	    }
      // 	  }
      // 	}
      // )
    // }

    stage("Check") {
      sh 'echo Check dashel_DIR is ${dashel_DIR}'
    }

    // stage("Enki") {
    //   unstash 'source'
    //   CMake([buildType: 'Debug',
    // 	     sourceDir: '$workDir/enki',
    // 	     buildDir: '$workDir/_build/enki/',
    // 	     installDir: '$workDir/_install',
    // 	     getCmakeArgs: [ '-DBUILD_SHARED_LIBS:BOOL=ON' ]
    // 	    ])
    // }

    // stage("Aseba") {
    //   unstash 'source'
    //   sh 'echo Aseba dashel_DIR is ${dashel_DIR}'
    //   CMake([buildType: 'Debug',
    // 	     sourceDir: '$workDir/aseba',
    // 	     buildDir: '$workDir/_build/aseba/',
    // 	     installDir: '$workDir/_install'
    // 	     // envs: [ "dashel_DIR=${env.dashel_DIR}" ]
    // 	    ])
    // }

    // stage("Archive") {
    //   archiveArtifacts artifacts: '_install/**', fingerprint: true, onlyIfSuccessful: true
    // }
    
  }
}
