pipeline {
  agent any

  tools {
    jdk 'jdk-11'
    maven 'mvn-3.6.3'
  }

  stages {
    stage('Clonar repositorio') {
      steps {
        git branch: 'main', url: 'https://github.com/FreddieAbad/DisenoArquitecturaSistemas.git'
      }
    }

    stage('Build') {
      steps {
        sh "mvn package"
        
      }
    }

    stage ('OWASP Dependency-Check Vulnerabilities') {
      steps {
        sh 'mvn dependency-check:check'

        dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
      }
    }

    stage ('PMD SpotBugs') {
      steps {
        sh 'mvn pmd:pmd pmd:cpd spotbugs:spotbugs'

        recordIssues enabledForFailure: true, tool: spotBugs()
        recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
        recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
      }
    }

    stage ('ZAP') {
      steps {
        sh 'mvn zap:analyze'
        publishHTML (target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'target/zap-reports',
          reportFiles: 'zapReport.html',
          reportName: "ZAP report"
          ])
      }
    }

    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv(credentialsId: 'sonarqube-secret', installationName: 'sonarqube-server') {
          sh 'mvn sonar:sonar -Dsonar.dependencyCheck.jsonReportPath=target/dependency-check-report.json -Dsonar.dependencyCheck.xmlReportPath=target/dependency-check-report.xml -Dsonar.dependencyCheck.htmlReportPath=target/dependency-check-report.html -Dsonar.java.pmd.reportPaths=target/pmd.xml -Dsonar.java.spotbugs.reportPaths=target/spotbugsXml.xml -Dsonar.zaproxy.reportPath=target/zap-reports/zapReport.xml -Dsonar.zaproxy.htmlReportPath=target/zap-reports/zapReport.html'
        }
      }
    }
  }
}
