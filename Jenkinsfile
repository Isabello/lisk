node('node-01'){
  lock(resource: "node-01", inversePrecedence: true) {
    stage ('Prepare Workspace') {
      sh '''#!/bin/bash
      pkill -f app.js || true
      dropdb lisk_test || true
      createdb lisk_test
      '''
      deleteDir()
      checkout scm
    }

    stage ('Build Dependencies') {
      try {
        sh '''#!/bin/bash

        # Install Deps
        npm install

        # Install Nodejs
        tar -zxf ~/lisk-node-Linux-x86_64.tar.gz
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, installation failed')
      }
    }

    stage ('Build submodules') {
      try {
        sh '''#!/bin/bash
        git submodule init
        git submodule update
        cd public/
        npm install
        bower install
        grunt release
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, grunt failed')
      }
    }

    stage ('Setup test environemnt') {
      try {
        sh '''#!/bin/bash
        cd test/lisk-js/; npm install; cd ../..
        cp test/config.json test/genesisBlock.json .
        export NODE_ENV=test
        BUILD_ID=dontKillMe ~/start_lisk.sh
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, Lisk failed')
      }
    }

    stage ('Parallel Tests') {
      parallel(
        "ESLint" : {
          sh '''
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run eslint
          '''
        },
        "Functional Accounts" : {
          sh '''
          export TEST=test/api/accounts.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Blocks" : {
          sh '''
          export TEST=test/api/blocks.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Delegates" : {
          sh '''
          export TEST=test/api/delegates.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Dapps" : {
          sh '''
          export TEST=test/api/dapps.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Loader" : {
          sh '''
          export TEST=test/api/loader.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Multisignatures" : {
          sh '''
          export TEST=test/api/multisignatures.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Signatures" : {
          sh '''
          export TEST=test/api/signatures.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Transactions" : {
          sh '''
          export TEST=test/api/transactions.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
      })
    }
/*
    stage ('Gather Coverage') {
      sh '''#!/bin/bash
      export HOST=127.0.0.1:4000

      npm run fetchCoverage

      # Submit coverage reports to Master
      scp test/.coverage-func.zip master-01:/var/lib/jenkins/coverage/coverage-func-node-01.zip
      '''
    }
*/
    stage ('Node-01 Cleanup') {
      sh '''
      pkill -f app.js -9
      '''
      }

    stage ('Set milestone') {
      milestone 1
      currentBuild.result = 'SUCCESS'
    }
  }
}

node('node-02'){
  lock(resource: "node-02", inversePrecedence: true) {
    stage ('Prepare Workspace') {
      sh '''#!/bin/bash
      pkill -f app.js || true
      dropdb lisk_test || true
      createdb lisk_test
      '''
      deleteDir()
      checkout scm
    }

    stage ('Build Dependencies') {
      try {
        sh '''#!/bin/bash

        # Install Deps
        npm install

        # Install Nodejs
        tar -zxf ~/lisk-node-Linux-x86_64.tar.gz
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, installation failed')
      }
    }

    stage ('Build submodules') {
      try {
        sh '''#!/bin/bash
        git submodule init
        git submodule update
        cd public/
        npm install
        bower install
        grunt release
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, grunt failed')
      }
    }

    stage ('Setup test environemnt') {
      try {
        sh '''#!/bin/bash
        cd test/lisk-js/; npm install; cd ../..
        cp test/config.json test/genesisBlock.json .
        export NODE_ENV=test
        BUILD_ID=dontKillMe ~/start_lisk.sh
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, Lisk failed')
      }
    }

    stage ('Parallel Tests') {
      parallel(
        "Functional Peer - Peer" : {
          sh '''
          export TEST=test/api/peer.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Dapp" : {
          sh '''
          export TEST=test/api/peer.dapp.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Blocks" : {
          sh '''
          export TEST=test/api/peer.blocks.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Signatures" : {
          sh '''
          export TEST=test/api/peer.signatures.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Transactions Collision" : {
          sh '''
          export TEST=test/api/peer.transactions.collision.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Transactions Delegates" : {
          sh '''
          export TEST=test/api/peer.transactions.delegates.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Transactions Main" : {
          sh '''
          export TEST=test/api/peer.transactions.main.js  TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Transaction Signatures" : {
          sh '''
          export TEST=test/api/peer.transactions.signatures.js  TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Peers" : {
          sh '''
          export TEST=test/api/peers.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        },
        "Functional Peer - Votes" : {
          sh '''
          export TEST=test/api/peer.transactions.votes.js TEST_TYPE='FUNC'
          cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
          npm run jenkins
          '''
        }
      )
    }
/*
    stage ('Gather Coverage') {
      sh '''#!/bin/bash
      export HOST=127.0.0.1:4000

      npm run fetchCoverage

      # Submit coverage reports to Master
      scp test/.coverage-func.zip master-01:/var/lib/jenkins/coverage/coverage-func-node-02.zip
      '''
    }
*/
    stage ('Node-02 Cleanup') {
      sh '''
      pkill -f app.js -9
      '''
      }

    stage ('Set milestone') {
      milestone 2
      currentBuild.result = 'SUCCESS'
    }
  }
}


node('node-03'){
  lock(resource: "node-03", inversePrecedence: true) {
    stage ('Prepare Workspace') {
      sh '''#!/bin/bash
      pkill -f app.js || true
      dropdb lisk_test || true
      createdb lisk_test
      '''
      deleteDir()
      checkout scm
    }

    stage ('Build Dependencies') {
      try {
        sh '''#!/bin/bash

        # Install Deps
        npm install

        # Install Nodejs
        tar -zxf ~/lisk-node-Linux-x86_64.tar.gz
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, installation failed')
      }
    }

    stage ('Build submodules') {
      try {
        sh '''#!/bin/bash
        git submodule init
        git submodule update
        cd public/
        npm install
        bower install
        grunt release
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, Grunt failed')
      }
    }

    stage ('Setup test environemnt') {
      try {
        sh '''#!/bin/bash
        cd test/lisk-js/; npm install; cd ../..
        cp test/config.json test/genesisBlock.json .
        export NODE_ENV=test
        BUILD_ID=dontKillMe ~/start_lisk.sh
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, Lisk failed')
      }
    }

    stage ('Parallel Tests') {
      parallel(
        "Unit - Helpers" : {
           sh '''
           export TEST=test/unit/helpers TEST_TYPE='UNIT'
           cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
           npm run jenkins
           '''
        },
        "Unit - Modules" : {
           sh '''
           export TEST=test/unit/modules TEST_TYPE='UNIT'
           cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
           npm run jenkins
           '''
        },
        "Unit - SQL" : {
           sh '''
           export TEST=test/unit/sql TEST_TYPE='UNIT'
           cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
           npm run jenkins
           '''
        },
        "Unit - Logic" : {
           sh '''
           export TEST=test/unit/logic TEST_TYPE='UNIT'
           cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
           npm run jenkins
           '''
        },
        "Functional Stress - Transactions" : {
           sh '''
           export TEST=test/api/peer.transactions.stress.js TEST_TYPE='FUNC'
           cd "$(echo $WORKSPACE | cut -f 1 -d '@')"
           npm run jenkins
           '''
        }
      )
    }
/*
    stage ('Gather Coverage') {
      sh '''#!/bin/bash
      export HOST=127.0.0.1:4000

      npm run fetchCoverage

      # Submit coverage reports to Master
      scp test/.coverage-unit/lcov.info master-01:/var/lib/jenkins/coverage/.coverage-unit/lcov.info
      scp test/.coverage-func.zip master-01:/var/lib/jenkins/coverage/coverage-func-node-03.zip
      '''
    }
*/
    stage ('Node-03 Cleanup') {
      sh '''
      pkill -f app.js -9
      '''
      }

    stage ('Set milestone') {
      milestone 2
      currentBuild.result = 'SUCCESS'
    }
  }
}
