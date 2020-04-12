  podTemplate(
   containers: [containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)],
   volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
   node(POD_LABEL) {
    def tag = "${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    def service = "market-data:${tag}"
    checkout scm
    container('docker') {
      stage('Build') {
        sh("docker build -t ${service} .")
      }
      stage('Test') {
        try {
          sh("docker run -v `pwd`:/workspace --rm ${service} python setup.py test")
        } finally {
          step([$class: 'JUnitResultArchiver', testResults: 'results.xml'])
        }
      }
      def tagToDeploy = "[w0wka91]/${service}"
      stage('Publish') {
        withDockerRegistry(registry: [credentialsId: 'dockerhub']) {
          sh("docker tag ${service} ${tagToDeploy}")
          sh("docker push ${tagToDeploy}")
        }
      }
    }
   }
  }