node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'
    def deliverDockerImage = 'cdrx/pyinstaller-linux:python2'
    def dockerArgs = '--cpus=2 --memory=2g --memory-swap=1g'

    stage('Build') {
        docker.image(buildDockerImage).inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image(testDockerImage).inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }

    stage('Deliver') {
        docker.image(deliverDockerImage).inside(dockerArgs) {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        archiveArtifacts 'dist/add2vals'
    }
}