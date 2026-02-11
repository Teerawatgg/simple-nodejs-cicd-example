pipeline {
  environment {
    VERCEL_PROJECT_NAME = 'simple-nodejs'
    VERCEL_TOKEN = credentials('devops14-github-token') // ดึงจาก Jenkins
  }
  // agent {
  //   kubernetes {
  //     // This YAML defines the "Docker Container" you want to use
  //     yaml '''
  //       apiVersion: v1
  //       kind: Pod
  //       spec:
  //         containers:
  //         - name: my-builder  # We will refer to this name later
  //           image: node:20-alpine
  //           command:
  //           - cat
  //           tty: true
  //     '''
  //   }
  agent {
    docker {
      image 'node:20-alpine'
      args '-u root:root'
    }
  }
  }
  stages {
    stage('Test npm') {
      steps {
        container('my-builder') {
          sh 'npm --version'
          sh 'node --version'
        }
      }
    }
    stage('Build') {
      steps {
        container('my-builder') {
          sh 'npm ci'
          sh 'npm run build'
        }
      }
    }
    stage('Test Build') {
      steps {
        container('my-builder') {
          sh 'npm run test'
        }
      }
    }
    stage('Deploy') {
      steps {
        container('my-builder') {
          sh 'npm install -g vercel@latest'
          // Deploy using token-only (non-interactive)
          sh '''
            vercel link --project $VERCEL_PROJECT_NAME --token $VERCEL_TOKEN --yes
            vercel --token $VERCEL_TOKEN --prod --confirm
          '''
        }
      }
    }

  }
}


// pipeline {
//   agent {
//     docker {
//       image 'node:20-alpine'
//       args '-u root:root'
//     }
//   }

//   environment {
//     VERCEL_PROJECT_NAME = 'simple-nodejs'
//     VERCEL_TOKEN = credentials('vercel-token')
//   }

//   stages {
//     stage('Test npm') {
//       steps {
//         sh 'node --version'
//         sh 'npm --version'
//       }
//     }

//     stage('Install') {
//       steps {
//         sh 'npm ci'
//       }
//     }

//     stage('Build') {
//       steps {
//         sh 'npm run build'
//       }
//     }

//     stage('Test Build') {
//       steps {
//         sh 'npm test'
//       }
//     }

//     stage('Deploy') {
//       steps {
//         withCredentials([string(credentialsId: 'vercel-token', variable: 'VERCEL_TOKEN')]) {
//           sh 'npm i -g vercel@latest'
//           sh '''
//             vercel link --project "$VERCEL_PROJECT_NAME" --token "$VERCEL_TOKEN" --yes
//             vercel --token "$VERCEL_TOKEN" --prod --confirm
//           '''
//         }
//       }
//     }
//   }
// }

