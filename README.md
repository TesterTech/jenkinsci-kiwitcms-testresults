# jenkinsci-kiwitcms-testresults
Code for the YouTube video https://youtu.be/piBJlrgzrMw

## References
- Jenkins CI https://www.jenkins.io/download/
- Plugin Demo https://github.com/kiwitcms/plugin-demo
- Variables available in Jenkins https://wiki.jenkins.io/display/JENKINS/Building+a+software+project#Buildingasoftwareproject-belowJenkinsSetEnvironmentVariables

## Pipeline
```
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git 'https://github.com/kiwitcms/plugin-demo.git'
                sh '''
                python3 -m venv plugin-demo
                . plugin-demo/bin/activate
                pip install wheel
                pip install -r requirements.txt
                export SSL_CERT_FILE=/home/demo/kiwi-tcms/ca.crt
                export TCMS_PRODUCT_VERSION=$JOB_NAME
                nosetests --with-tap --tap-stream 2> output.tap
                tcms-tap-plugin output.tap

                nosetests --with-xunit
                tcms-junit.xml-plugin nosetests.xml
                '''
            }
        }
    }
}
```
