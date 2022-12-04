node {
    docker.image('python:2-alpine').inside {
        stage('Build') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    docker.image('qnib/pytest').inside {
        stage('Test') {
            checkout scm
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    docker.image('cdrx/pyinstaller-linux:python2').inside {
        stage('Deliver') {
            checkout scm
            sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-wilson_oey/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
            archiveArtifacts artifacts: 'sources/add2vals.py', followSymlinks: false
            sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-wilson_oey/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
        }
    }
}