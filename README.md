# To create a Jenkins pipeline at Openshif



## Login

```ruby
oc login -u developer -p 123
```



## Create a demo project

```ruby
oc new-project hello-openshift-jenkins-ci-cd
```



## create file BuildConfig :

```json
apiVersion: v1
kind: BuildConfig
metadata:
  name: hello-openshift-jenkins-ci-cd
  labels:
    name: hello
spec:
  source:       
    git:   
      ref: master       
      uri: https://github.com/jovemfelix/hello-openshift-jenkins-ci-cd
  runPolicy: Serial
  strategy:
    type: JenkinsPipeline
    jenkinsPipelineStrategy:
      jenkinsfilePath: Jenkinsfile

kind: ConfigMap
apiVersion: v1
metadata:
  name: jenkins-agent
  labels:
    role: jenkins-slave
data:
  template1: |-
    <org.csanchez.jenkins.plugins.kubernetes.PodTemplate>
      <inheritFrom></inheritFrom>
      <name>template1</name>
      <instanceCap>2147483647</instanceCap>
      <idleMinutes>0</idleMinutes>
      <label>template1</label>
      <serviceAccount>jenkins</serviceAccount>
      <nodeSelector></nodeSelector>
      <volumes/>
      <containers>
        <org.csanchez.jenkins.plugins.kubernetes.ContainerTemplate>
          <name>jnlp</name>
          <image>openshift/jenkins-agent-maven-35-centos7:v3.10</image>
          <privileged>false</privileged>
          <alwaysPullImage>true</alwaysPullImage>
          <workingDir>/tmp</workingDir>
          <command></command>
          <args>${computer.jnlpmac} ${computer.name}</args>
          <ttyEnabled>false</ttyEnabled>
          <resourceRequestCpu></resourceRequestCpu>
          <resourceRequestMemory></resourceRequestMemory>
          <resourceLimitCpu></resourceLimitCpu>
          <resourceLimitMemory></resourceLimitMemory>
          <envVars/>
        </org.csanchez.jenkins.plugins.kubernetes.ContainerTemplate>
      </containers>
      <envVars/>
      <annotations/>
      <imagePullSecrets/>
      <nodeProperties/>
    </org.csanchez.jenkins.plugins.kubernetes.PodTemplate>
```



### To create run

```ruby
oc apply -f jenkins.yaml
```



## Reference

- https://opensource.com/article/18/11/best-practices-cicd
- https://www.infoq.com/articles/scaling-docker-with-kubernetes
- https://github.com/jenkinsci/kubernetes-plugin