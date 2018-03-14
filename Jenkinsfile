#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String jenkinsPackageName = ""
String jenkinsStoragePackageName = ""
String jenkinsChartName = "jenkins"
String jenkinsStorageChartName = "jenkins-storage"

toolsNode(toolsImage: 'stakater/pipeline-tools:1.2.0') {
    container(name: 'tools') {
        def helm = new io.stakater.charts.Helm()
        def common = new io.stakater.Common()
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
            String cmUsername = common.getEnvValue('CHARTMUSEUM_USERNAME')
            String cmPassword = common.getEnvValue('CHARTMUSEUM_PASSWORD')
            chartManager.uploadToChartMuseum(WORKSPACE, jenkinsChartName, jenkinsPackageName, cmUsername, cmPassword)
            chartManager.uploadToChartMuseum(WORKSPACE, jenkinsStorageChartName, jenkinsStoragePackageName, cmUsername, cmPassword)
        }
    }
}
