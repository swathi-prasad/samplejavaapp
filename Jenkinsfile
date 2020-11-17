pipeline {
  agent any
  stages {
    stage('compile') {
      steps {
        git 'https://github.com/lerndevops/samplejavaapp.git'
        sh '/opt/apache-maven-3.6.3/bin/mvn compile'
        sleep 10
      }
    }

    stage('codereview-pmd') {
      
      steps {
        sh '/opt/apache-maven-3.6.3/bin/mvn -P metrics pmd:pmd'
      }
    }

    stage('unit-test') {
      post {
        success {
          junit 'target/surefire-reports/*.xml'
        }

      }
      steps {
        sh '/opt/apache-maven-3.6.3/bin/mvn test'
      }
    }

    stage('codecoverage') {
      post {
        success {
          cobertura(autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false)
        }

      }
      steps {
        sh '/opt/apache-maven-3.6.3/bin/mvn cobertura:cobertura -Dcobertura.report.format=xml'
      }
    }

    stage('package') {
      steps {
        sh '/opt/apache-maven-3.6.3/bin/mvn clean package'
      }
    }

  }
}
