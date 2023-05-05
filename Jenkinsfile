pipeline {
    agent any

    stages {
        stage('Record compiler warnings and static analysis results') {
            steps {
                recordIssues enabledForFailure: true,
                tool: parasoftFindings(
                    pattern: '**/*static*.xml,**/*metric*.xml,**/*flowanalysis*.xml',
                    skipSymbolicLinks: false,
                    reportEncoding: 'UTF-8',
                    localSettingsPath: 'settings/settings.properties',
                    name: 'Parasoft Findings Dev'
                    )
            }
        }

        stage('Publish xUnit test result report - ParasoftSOAtest-9.x'){
            steps {
                step([$class: 'XUnitPublisher',
                    tools: [
                        [$class: 'ParasoftSOAtest9xType',
                            pattern: '**/soatestDesktop9Functional*.xml,**/SOAtest_functional_10.6.1.xml,**/soatestDesktopCalc3.xml,**/soatestWar*.xml,**/soatest_10.5.0.xml,**/soatest_desktop_10_5_2.xml',
                            skipNoTestFiles: false,
                            failIfNotNew: false,
                            deleteOutputFiles: true,
                            stopProcessingIfError: true
                        ]
                    ]
                ])
            }
        }

        stage('Publish xUnit test result report - ParasoftAnalyzers-10.x'){
            steps {
                step([$class: 'XUnitPublisher',
                    tools: [
                        [$class: 'ParasoftType',
                            pattern: '**/*unit*.xml',
                            skipNoTestFiles: false,
                            failIfNotNew: false,
                            deleteOutputFiles: true,
                            stopProcessingIfError: true
                        ]
                    ]
                ])
            }
        }
    }
}
