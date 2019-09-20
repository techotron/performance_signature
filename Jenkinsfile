pipeline {
  agent {
    label 'docker'
  }
  options {
    timestamps()
    ansiColor('xterm')
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        script {
          println "Checkout my code"
        }
      }
    }
    stage('Run tests') {
      steps {
        script {
          def up = sh label: '', returnStdout: true, script: 'echo $RANDOM'
          def down = sh label: '', returnStdout: true, script: 'echo $RANDOM'

          def perfTests = [['P50', 'P95', 'P99', 'P100'], ["6","12","8","45"]]
          writeCSV file: 'perf-test.csv', records: perfTests, format: CSVFormat.EXCEL

          def speedTests = [['up','down'], [up, down]]
          writeCSV file: 'speed-test.csv', records: speedTests, format: CSVFormat.EXCEL
        }
      }
    }
    stage('Plot Graphs') {
      steps {
        script {
          def perfTestCsv = "plot-1207576f-8989-4c7e-9ec9-48e4564d8e0d.csv"
          def speedTestCsv = "plot-67d8b5cc-d0cb-4af4-a047-5cc10fc3ba20.csv"

          plot csvFileName: perfTestCsv,
              csvSeries: [[
                              width: 1600,
                              height: 1600,
                              displayTableFlag: false,
                              exclusionValues: '',
                              file: 'perf-test.csv',
                              inclusionFlag: 'OFF',
                              url: '']],
              group: 'Performance Signatures',
              keepRecords: true,
              numBuilds: '50',
              style: 'line',
              title: 'Performance Test',
              yaxis: 'Percent (%)'

          plot csvFileName: speedTestCsv,
              csvSeries: [[
                              width: 1600,
                              height: 1600,
                              displayTableFlag: false,
                              exclusionValues: '',
                              file: 'speed-test.csv',
                              inclusionFlag: 'OFF',
                              url: '']],
              group: 'Performance Signatures',
              keepRecords: true,
              numBuilds: '50',
              style: 'line',
              title: 'BB Speed Test',
              yaxis: 'Speed(mbps)'
        }
      }
    }
  }
}
