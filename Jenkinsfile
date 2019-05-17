@Library('retort-lib') _
def label = "jenkins-${UUID.randomUUID().toString()}"
 
def ZCP_USERID = 'cloudzcp-admin'
def DOCKER_IMAGE = 'lay126/harbor-private-img'
def K8S_NAMESPACE = 'zcp-system'
def VERSION = 'develop'
def ZCP-DOMAIN = 'pog-dev'
 
podTemplate(label:label,
    serviceAccount: "zcp-system-sa-${ZCP_USERID}",
    containers: [
        containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat', envVars: [
            envVar(key: 'DOCKER_HOST', value: 'tcp://jenkins-dind-service:2375 ')]),
        containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat')
    ],
    volumes: [
        persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'zcp-jenkins-mvn-repo')
    ]) {
 
    node(label) {
        stage('BUILD DOCKER IMAGE') {
            container('docker') {
                sh "docker login ${ZCP-DOMAIN}-registry.cloudzcp.io"

                sh "docker pull cloudzcp/zcp-iam:1.2.0-beta"
                sh "docker tag cloudzcp/zcp-iam:1.2.0-beta ${ZCP-DOMAIN}-registry.cloudzcp.io/cloudzcp/zcp-iam:1.2.0"
                sh "docker push ${ZCP-DOMAIN}-registry.cloudzcp.io/cloudzcp/zcp-iam:1.2.0"
             }
        }
    }
}
