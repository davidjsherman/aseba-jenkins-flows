#!groovy

def CMake = fileLoader.fromGit('vars/CMake', 'https://github.com/davidjsherman/aseba-jenkins.git', 'master', null, '')

stage 'Setup'
node() {
  git branch: 'pr17-playground', url: 'https://github.com/davidjsherman/enki.git'
  stash excludes: '.git', name: 'source'
}

def labels = ['inirobot-deb64', 'inirobot-u64', 'inirobot-win7', 'inirobot-osx']
def builders = [:]		// map to fire all builds in parallel
for (x in labels) {
  def label = x			// Need to bind the label variable before the closure
  def output = "enki-" + x
  
  builders[label] = {
    node(label) {
      unstash 'source'
      CMake.call([buildType: 'Debug',
		  buildDir: '$workDir/_build',
		  installDir: '$workDir/_install/' + output,
		  getCmakeArgs: '-DBUILD_SHARED_LIBS:BOOL=OFF'
		 ])
      stash includes: '_install/'+output+'/**', name: output
      sh "mkdir -p _install/${output}/bin && install _build/examples/enkiTest _build/viewer/enkiplayground _install/${output}/bin"
      archiveArtifacts artifacts: '_install/'+output+'/**', fingerprint: true, onlyIfSuccessful: true
    }
  }
}

stage 'Build'
parallel builders
