apiVersion: v1
kind: BuildConfig
metadata:
  name: hello-jenkins-cicd
  labels:
    name: hello-jenkins-cicd
spec:
  runPolicy: Serial
  strategy:
    type: JenkinsPipeline
    jenkinsPipelineStrategy:
      jenkinsfile: |-
        pipeline {
            agent any
            stages {
                stage('Build') {
                    steps {
                        echo 'Pipeline is running
                    }
                }
            }
        }