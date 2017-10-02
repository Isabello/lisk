def initBuild() {
	sh '''#!/bin/bash
	pkill -f app.js -9 || true
	sudo service postgresql restart
	dropdb lisk_test || true
	createdb lisk_test
	'''
	deleteDir()
	checkout scm
}

def buildDependency() {
	try {
		sh '''#!/bin/bash
		# Install Deps
		npm install
		'''
	} catch (err) {
		currentBuild.result = 'FAILURE'
		report()
		error('Stopping build, installation failed')
	}
}

def startLisk() {
	try {
		sh '''#!/bin/bash
		cp test/config.json test/genesisBlock.json .
		export NODE_ENV=test
		JENKINS_NODE_COOKIE=dontKillMe ~/start_lisk.sh
		'''
	} catch (err) {
		currentBuild.result = 'FAILURE'
		report()
		error('Stopping build, Lisk failed')
	}
}

def report(){
	step([
		$class: 'GitHubCommitStatusSetter',
		errorHandlers: [[$class: 'ShallowAnyErrorHandler']],
		contextSource: [$class: 'ManuallyEnteredCommitContextSource', context: 'jenkins-ci/func-unit'],
		statusResultSource: [
			$class: 'ConditionalStatusResultSource',
			results: [
					[$class: 'BetterThanOrEqualBuildResult', result: 'SUCCESS', state: 'SUCCESS', message: 'This commit looks good :)'],
					[$class: 'BetterThanOrEqualBuildResult', result: 'FAILURE', state: 'FAILURE', message: 'This commit failed testing :('],
					[$class: 'AnyBuildResult', state: 'FAILURE', message: 'This build some how escaped evaluation']
			]
		]
	])
}

lock(resource: "Lisk-Core-Nodes-New", inversePrecedence: true) {
	stage ('Prepare Workspace') {
		parallel(
			"Build node-11" : {
				node('node-11'){
					initBuild()
				}
			},
			"Build node-12" : {
				node('node-12'){
					initBuild()
				}
			},
			"Build node-13" : {
				node('node-13'){
					initBuild()
				}
			},
			"Build node-14" : {
				node('node-14'){
					initBuild()
				}
			}
		)
	}

	stage ('Build Dependencies') {
		parallel(
			"Build Dependencies node-11" : {
				node('node-11'){
					buildDependency()
				}
			},
			"Build Dependencies node-12" : {
				node('node-12'){
					buildDependency()
				}
			},
			"Build Dependencies node-13" : {
				node('node-13'){
					buildDependency()
				}
			},
			"Build Dependencies node-14" : {
				node('node-14'){
					buildDependency()
				}
			}
		)
	}

	stage ('Start Lisk') {
		parallel(
			"Start Lisk node-11" : {
				node('node-11'){
					startLisk()
				}
			},
			"Start Lisk node-12" : {
				node('node-12'){
					startLisk()
				}
			},
			"Start Lisk node-13" : {
				node('node-13'){
					startLisk()
				}
			},
			"Start Lisk node-14" : {
				node('node-14'){
					startLisk()
				}
			}
		)
	}

	stage ('Parallel Tests') {
		parallel(
			"ESLint" : {
				node('node-11'){
					sh '''
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run eslint
					'''
				}
			},
			"Functional Accounts" : {
				node('node-11'){
					sh '''
					export TEST=test/api/accounts.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Blocks" : {
				node('node-11'){
					sh '''
					export TEST=test/api/blocks.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Delegates" : {
				node('node-11'){
					sh '''
					export TEST=test/api/delegates.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Dapps" : {
				node('node-11'){
					sh '''
					export TEST=test/api/dapps.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Loader" : {
				node('node-11'){
					sh '''
					export TEST=test/api/loader.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transport - Handshake" : {
				node('node-12'){
					sh '''
					export TEST=test/api/transport/transport.handshake.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Multisignatures" : {
				node('node-11'){
					sh '''
					export TEST=test/api/multisignatures.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Multisignatures POST" : {
				node('node-11'){
					sh '''
					export TEST=test/api/multisignatures.post.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transactions" : {
				node('node-11'){
					sh '''
					export TEST=test/api/transactions.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			}, //End node-11 functional tests
			"Functional Transport - Main" : {
				node('node-12'){
					sh '''
					export TEST=test/api/transport/transport.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transport - Blocks" : {
				node('node-12'){
					sh '''
					export TEST=test/api/transport/transport.blocks.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transport - Client" : {
				node('node-12'){
					sh '''
					export TEST=test/api/transport/transport.client.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transport - Transactions Main" : {
				node('node-12'){
					sh '''
					export TEST=test/api/transport/transport.transactions.main.js  TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},
			"Functional Transport - Peers" : {
				node('node-12'){
					sh '''
					export TEST=test/api/peers.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			},  // End node-12 functional tests
			"Unit Tests" : {
				node('node-13'){
					sh '''
					export TEST_TYPE='UNIT' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run test-unit
					'''
				}
			}, // End node-13 unit tests
			"Functional Stress - Transactions" : {
				node('node-14'){
					sh '''
					export TEST=test/api/transport/transport.transactions.stress.js TEST_TYPE='FUNC' NODE_ENV='TEST'
					cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
					npm run jenkins
					'''
				}
			}
		) // End Parallel
	}

	stage ('Set milestone') {
		node('master-01'){
			milestone 1
			currentBuild.result = 'SUCCESS'
			report()
		}
	}
}
