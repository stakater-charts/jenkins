#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String jenkinsPackageName = ""
String jenkinsStoragePackageName = ""
String jenkinsChartName = "jenkins"
String jenkinsStorageChartName = "jenkins-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        def helm = new io.stakater.charts.Helm()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }
        
        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, jenkinsChartName)
            jenkinsPackageName = helm.package(WORKSPACE, jenkinsChartName)

            helm.lint(WORKSPACE, jenkinsStorageChartName)
            jenkinsStoragePackageName = helm.package(WORKSPACE, jenkinsStorageChartName)
        }

        stage('Upload Chart') {
            chartManager.uploadToChartMuseum(WORKSPACE, jenkinsChartName, jenkinsPackageName)
            chartManager.uploadToChartMuseum(WORKSPACE, jenkinsStorageChartName, jenkinsStoragePackageName)
        }
    }
}
