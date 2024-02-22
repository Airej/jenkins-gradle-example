pipeline {
  agent {
    docker {
      args '-v /jenkins/.gradle:/jenkins/.gradle'
      image 'gradle:latest'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh './gradlew :build'
      }
    }
    stage('Test') {
      steps {
        sh './gradlew :test'
        junit 'build/test-results/test/*xml'
        sh './gradlew :cobertura'
      }
      // Send Coverage results
      post{
        always{
          step([$class: 'CoberturaPublisher',
                         autoUpdateHealth: false,
                         autoUpdateStability: false,
                         coberturaReportFile: 'build/reports/cobertura/coverage.xml',
                         failNoReports: false,
                         failUnhealthy: false,
                         failUnstable: false,
                         maxNumberOfBuilds: 10,
                         onlyStable: false,
                         sourceEncoding: 'ASCII',
                         zoomCoverageChart: false])
        }
      }
    }
    stage('Static Tests') {
      steps {
        sh './gradlew :check'
        recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'build/reports/checkstyle/*.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: cpd(pattern: 'build/reports/cpd/*.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'build/reports/pmd/*.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'build/reports/spotbugs/*xml'), sourceCodeEncoding: 'UTF-8'
      }
    }
  }
  environment {
    GRADLE_USER_HOME = '/jenkins/.gradle'
  }
}


// pipeline {
//     agent any

//     stages {
//         stage('Print Versions') {
//             steps {
//                 script {
//                     sh "/var/lib/jenkins/.gradle/wrapper/dists/gradle-6.4.1-all/13imxtezgn9nwzqt8rgtkunh1/gradle-6.4.1/bin/gradle --version"
                    
//                     // Print Java version
//                     sh 'java -version'
//                 }
//             }
//         }
//     }
// }
