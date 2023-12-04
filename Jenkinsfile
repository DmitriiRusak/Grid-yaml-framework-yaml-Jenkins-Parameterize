pipeline{
	
	agent any
	
	parameters {
		choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
	}
	
	stages{
		
		stage('Start grid'){
			steps{
				bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
			}
		}
		
		stage('Run tests'){
			steps{
				bat "docker-compose -f test_suite.yaml up"			
			}
		}
	}

	post {
		always {
			bat "docker-compose -f grid.yaml down"
			bat "docker-compose -f test_suite.yaml down"
			archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
			archiveArtifacts artifacts: 'output/vendorPortal/emailable-report.html', followSymlinks: false
		}
	
	}
}