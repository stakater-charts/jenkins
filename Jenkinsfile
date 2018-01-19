#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String jenkinsPackageName = ""
String jenkinsStoragePackageName = ""
String jenkinsChartName = "jenkins"
String jenkinsStorageChartName = "jenkins-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            sh "helm init --client-only"
        }

        stage('Prepare Chart') {
            jenkinsPackageName = prepareChart(jenkinsChartName)
            jenkinsStoragePackageName = prepareChart(jenkinsStorageChartName)
        }

        stage('Upload Chart') {
            uploadChart(jenkinsChartName, jenkinsPackageName)
            uploadChart(jenkinsStorageChartName, jenkinsStoragePackageName)
        }
    }
}

def prepareChart(String chartName) {
    result = shOutput """
                cd ${WORKSPACE}/${chartName}
                helm lint
                helm package .
            """

    return result.substring(result.lastIndexOf('/') + 1, result.length())
}

def uploadChart(String chartName, String fileName) {
    sh """
        cd ${WORKSPACE}/${chartName}
        curl -L --data-binary \"@${fileName}\" http://chartmuseum/api/charts
    """
}

def shOutput(String command) {
    return sh(
        script: """
            ${command}
        """,
        returnStdout: true).trim()
}