podTemplate(yaml: '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker
    command: ['cat']
    tty: true
'''
  ) {
  node(POD_LABEL) {
    def tag = "${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    def service = "market-data:${tag}"
    checkout scm
    stage('Build a Maven project') {
      container('docker') {
        sh("docker build -t ${service} .")
      }
    }
  }
}