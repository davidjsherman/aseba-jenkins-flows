#!groovy

stage('Dashel') {
	node('inirobot-win7') {
		git branch: 'pollsocketstream', credentialsId: '55b7beaa-040d-40cb-ba15-a03d1df32fca', url: 'https://github.com/davidjsherman/dashel.git'
		sh '''git submodule update --init'''
		withEnv(["WORKSPACE=${pwd()}","INSTALL=${pwd()}/_install","BUILD=${pwd()}/_build"]) {
			sh '''echo WORKSPACE=${WORKSPACE}
				rm -rf ${INSTALL}/dashel && mkdir -p ${INSTALL}/dashel
				rm -rf ${BUILD} && mkdir ${BUILD}
				cd ${BUILD}
				cmake ${WORKSPACE} -G 'Unix Makefiles' -DCMAKE_INSTALL_PREFIX:PATH=${INSTALL}/dashel \
					-DBUILD_SHARED_LIBS:BOOL=OFF
				make
				make install
			'''
		}
		stash includes: '_install/dashel/**', name: 'dashel'
	}
}
stage('Enki') {
	node('inirobot-win7') {
		git branch: 'old-api', credentialsId: '55b7beaa-040d-40cb-ba15-a03d1df32fca', url: 'https://github.com/davidjsherman/enki.git'
		sh '''git submodule update --init'''
		withEnv(["WORKSPACE=${pwd()}","INSTALL=${pwd()}/_install","BUILD=${pwd()}/_build"]) {
			sh '''echo WORKSPACE=${WORKSPACE}
				rm -rf ${INSTALL}/enki && mkdir -p ${INSTALL}/enki
				rm -rf ${BUILD} && mkdir ${BUILD}
				cd ${BUILD}
				cmake ${WORKSPACE} -G 'Unix Makefiles' -DCMAKE_INSTALL_PREFIX:PATH=${INSTALL}/enki
				make
				make install
			'''
		}
		stash includes: '_install/enki/**', name: 'enki'
	}
}
stage('Aseba') {
	node('inirobot-win7') {
		git branch: 'zeroconf-PR-derived', credentialsId: '55b7beaa-040d-40cb-ba15-a03d1df32fca', url: 'https://github.com/davidjsherman/aseba.git'
		sh '''git submodule update --init'''
		unstash 'dashel'
		unstash 'enki'
		withEnv(["WORKSPACE=${pwd()}","INSTALL=${pwd()}/_install","BUILD=${pwd()}/_build"]) {
			sh '''echo WORKSPACE=${WORKSPACE}
				rm -rf ${INSTALL}/aseba && mkdir -p ${INSTALL}/aseba
				rm -rf ${BUILD} && mkdir ${BUILD}
				cd ${BUILD}
				cmake ${WORKSPACE} -G 'Unix Makefiles' -DCMAKE_INSTALL_PREFIX:PATH=${INSTALL}/aseba \
					-Ddashel_DIR=${INSTALL}/dashel \
					-DDASHEL_LIBRARY:FILEPATH=${INSTALL}/dashel/lib/libdashel.a \
					-DDASHEL_INCLUDE_DIR:PATH=${INSTALL}/dashel/include \
					-DENKI_INCLUDE_DIR:PATH=${INSTALL}/enki/include \
					-DENKI_LIBRARY:FILEPATH=${INSTALL}/enki/lib/libenki.a \
					-DENKI_VIEWER_LIBRARY:FILEPATH=${INSTALL}/enki/lib/libenkiviewer.a
				make
				make install
			'''
		}
		stash includes: '_install/**', name: 'aseba'
	}
}
