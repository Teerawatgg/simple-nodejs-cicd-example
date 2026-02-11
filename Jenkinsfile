// pipeline {
//   environment {
//     VERCEL_PROJECT_NAME = 'simple-nodejs'
//     VERCEL_TOKEN = credentials('vercel-token') // ดึงจาก Jenkins
//   }
//   agent {
//     kubernetes {
//       // This YAML defines the "Docker Container" you want to use
//       yaml '''
//         apiVersion: v1
//         kind: Pod
//         spec:
//           containers:
//           - name: my-builder  # We will refer to this name later
//             image: node:20-alpine
//             command:
//             - cat
//             tty: true
//       '''
//     }
//   }
//   stages {
//     stage('Test npm') {
//       steps {
//         container('my-builder') {
//           sh 'npm --version'
//           sh 'node --version'
//         }
//       }
//     }
//     stage('Build') {
//       steps {
//         container('my-builder') {
//           sh 'npm ci'
//           sh 'npm run build'
//         }
//       }
//     }
//     stage('Test Build') {
//       steps {
//         container('my-builder') {
//           sh 'npm run test'
//         }
//       }
//     }
//     stage('Deploy') {
//       steps {
//         container('my-builder') {
//           sh 'npm install -g vercel@latest'
//           // Deploy using token-only (non-interactive)
//           sh '''
//             vercel link --project $VERCEL_PROJECT_NAME --token $VERCEL_TOKEN --yes
//             vercel --token $VERCEL_TOKEN --prod --confirm
//           '''
//         }
//       }
//     }

//   }
// }


pipeline {
  agent any

  environment {
    VERCEL_PROJECT_NAME = 'simple-nodejs'
    VERCEL_TOKEN = credentials('vercel-token')
  }

  stages {
    stage('Test npm') {
      steps {
        bat 'npm --version'
        bat 'node --version'
      }
    }

    stage('Build') {
      steps {
        bat 'npm ci'
        bat 'npm run build'
      }
    }

    stage('Test Build') {
      steps {
        bat 'npm test'
      }
    }

    stage('Deploy') {
      steps {
        bat 'npm i -g vercel@latest'
        bat """
          vercel link --project %VERCEL_PROJECT_NAME% --token %VERCEL_TOKEN% --yes
          vercel --token %VERCEL_TOKEN% --prod --confirm
        """
      }
    }
  }
}
