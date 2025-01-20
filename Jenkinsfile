node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'
    def deliverDockerImage = 'cdrx/pyinstaller-linux:python2'

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
        docker.image(deliverDockerImage).inside {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
        if (currentBuild.result == 'SUCCESS') {
        archiveArtifacts 'dist/add2vals'
    }
}