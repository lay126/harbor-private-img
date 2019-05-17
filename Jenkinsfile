@Library('retort-lib') _
def label = "jenkins-${UUID.randomUUID().toString()}"
 
def ZCP_USERID = 'cloudzcp-admin'
def K8S_NAMESPACE = 'zcp-system'
def VERSION = 'develop'
def ZCP_DOMAIN = 'pog-dev'
 
podTemplate(label:label,
    serviceAccount: "zcp-system-sa-${ZCP_USERID}",
    containers: [
        containerTemplate(name: 'docker', image: 'docker:17-dind', ttyEnabled: true, command: 'dockerd-entrypoint.sh', privileged: true),
        containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat')
    ],
    volumes: [
        persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'zcp-jenkins-mvn-repo')
    ]) {
 
    node(label) {
        stage('BUILD DOCKER IMAGE') {
            container('docker') {
                sh "docker login -u cloudzcp-admin -p Cloudzcp!23$ pog-dev-registry.cloudzcp.io"

                sh "docker pull cloudzcp/zcp-iam:1.2.0-beta"
                sh "docker tag cloudzcp/zcp-iam:1.2.0-beta ${ZCP_DOMAIN}-registry.cloudzcp.io/cloudzcp/zcp-iam:1.2.0"
                dockerCmd.push registry: 'pog-dev-registry.cloudzcp.io', imageName: 'cloudzcp/zcp-iam', imageVersion: '1.2.0', credentialsId: "HARBOR_CREDENTIALS"
             }
        }
    }
}
