  podTemplate(
   containers: [containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)],
   volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
   node(POD_LABEL) {
    def tag = "${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    def service = "market-data:${tag}"
    checkout scm
    stage('Build a Maven project') {
     container('docker') {
      sh("docker build -t ${service} .")
     }
    }
    stage('Test') {
      sh("docker run --rm ${service} python setup.py test")
    }
    stage('Test') {
      try {
        sh("docker run -v `pwd`:/workspace --rm ${service} python setup.py test")
      } finally {
        step([$class: 'JUnitResultArchiver', testResults: 'results.xml'])
      }
    }
   }
  }