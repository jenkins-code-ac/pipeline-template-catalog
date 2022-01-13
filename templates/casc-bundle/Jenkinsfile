library 'pipeline-library'
pipeline {
  agent none
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    timeout(time: 60, unit: 'MINUTES')
  }
  stages {
    stage('Update Config Bundle') {
      agent {
        kubernetes {
          yaml libraryResource ('podtemplates/kubectl.yml')
        }
      }
      when {
        beforeAgent true
        branch 'main'
      }
      steps {
        container("kubectl") {
          sh "mkdir -p ${BUNDLE_ID}"
          sh "find -name '*.yaml' | xargs cp --parents -t ${BUNDLE_ID}"
          sh "kubectl cp --namespace cbci ${BUNDLE_ID} cjoc-0:/var/jenkins_home/jcasc-bundles-store/ -c jenkins"
        }
      }
    }
  }
}
