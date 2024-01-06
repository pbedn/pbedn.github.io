+++
title = "Configure CI for Python app in Azure Devops (Pytest, Nexus IQ, SonarQube)"
date = "2024-01-05"
draft = false
tags =  ["azure", "nexusiq", "sonarqube", "python", "pytest"]
+++

Example of Azure Pipeline yaml file to configure pytest with code coverage and Nexus IQ / SonarQube scanners for Python application.

<!--more-->

Additional yaml file features:

* trigger CI on merge to master, or each tag
* pip download folder caching
* freeze requirements and add marker for selected platform as recommended in Nexux docs
* publish test result
* publish code coverage report using Cobertura tool

Notes:

* overall time save is neglible compared to regular *pip install* for small requirements file
* azure pipeline file grows quickly, so if using similar in multiple repositories it is worth to implement pipeline templates
* Sonar and Nexus are configured to run only on merge to master

Azure Devops configuration for SonarQube and Sonata Nexus:

* [Link to docs: Azure - SonarQube](https://docs.sonarsource.com/sonarqube/latest/devops-platform-integration/azure-devops-integration/)
* [Link to docs: Azure - Nexus IQ](https://help.sonatype.com/iqserver/integrations/plugins-for-continuous-integration-platforms/nexus-iq-for-azure-devops)

Example app structure:
```
src\
  core\
  core2\
  utils\
  resources\
tests\
  testcore\
  testsutils\
requirements.txt
```

Example Azure CI pipeline file:
```
# azure-pipeline.yml

trigger:
  branches:
    include:
      - master
  tags:
    include:
      - '*'

resources:
  repositories:
    - repository: self
      type: git
      name: <put-here-name-of-repository>

variables:
  pythonVersion: '3.11'
  pipDownloadDir: $(Pipeline.Workspace)/.pip

jobs:
  - job: Test
    pool:
      name: 'Azure Pipelines'
      vmImage: ubuntu-latest
    steps:
      - task: UsePythonVersion@0
        inputs:
          versionSpec: $(pythonVersion)
    
      - task: Cache@2
        displayName: Load cache
        inputs:
          key: 'pip | "$(Agent.OS)" | requirements.txt'
          path: $(pipDownloadDir)
          cacheHitVar: cacheRestored
    
      - script: pip download -r requirements.txt --dest=$(pipDownloadDir)
        displayName: 'Download requirements'
        condition: eq(variables.cacheRestored, 'false')

      - script: pip install -r requirements.txt --no-index --find-links=$(pipDownloadDir)
        displayName: 'Install requirements'

      - script: |
          mkdir -p dist
          pip freeze > frozen_req.txt
          cat frozen_req.txt | while read line; do echo ${line}'; sys_platform="linux"' >> frozen_req_sys.txt; done
          mv frozen_req_sys.txt dist/requirements.txt
        displayName: 'Prepare files for Nexus scanning'
    
      - script: pytest -vs --junitxml=junit/test-results.xml --cov=src --cov-report=xml tests/
        displayName: 'Run tests with coverage'

      - script: cp coverage.xml dist/coverage.xml
        displayName: 'Copy coverage report for Sonar'
    
      - task: PublishTestResults@1
        condition: succeededOrFailed()
        inputs:
          testResultsFiles: 'junit/**.xml'
          testRunTitle: 'Publish test results'

      - task: PublishCodeCoverageResults@1
        inputs:
          codeCoverageTool: Cobertura
          summaryFileLocation: '$(System.DefaultWorkingDirectory)/coverage.xml'
    
      - publish: $(System.DefaultWorkingDirectory)/dist
        artifact: dist
        displayName: 'Publish files for scanning'

  - job: Sonar
    dependsOn: Test
    condition: and(succeded(), eq(variables['build.sourceBranch'], 'refs/heads/master'))
    pool:
      name: 'Azure Pipelines'
      vmImage: ubuntu-latest
    steps:
      - task: DownloadPipelineArtifact@2
        inputs:
          source: current
          artifact: dist
          path: $(System.DefaultWorkingDirectory)
    
      - task: SonarQubePrepare@5
        inputs:
            SonarQube: 'YourSonarqubeServerEndpoint'
            scannerMode: 'Other'
            extraProperties: |
              sonar.projectKey=<your-application-name>.$(Build.Repository.Name) 
              sonar.verbose=true
              sonar.sources=src/
              sonar.tests=tests/
              sonar.exclusions=src/noncore/**,src/resources/**
              sonar.python.version=$(pythonVersion)
              sonar.python.coverage.reportPaths=coverage.xml
              sonar.coverage.exclusions=src/noncore/**/*,src/resources/**/*

        # Run Code Analysis task
        - task: SonarQubeAnalyze@5

        # Publish Quality Gate Result task
        - task: SonarQubePublish@5
          inputs:
            pollingTimeoutSec: '300'

  - job: NexusIQ
    dependsOn: Test
    condition: and(succeded(), eq(variables['build.sourceBranch'], 'refs/heads/master'))
    pool:
      name: 'Azure Pipelines'
      vmImage: ubuntu-latest
    steps:
      - task: DownloadPipelineArtifact@2
        inputs:
          source: current
          artifact: dist
          path: $(System.DefaultWorkingDirectory)
    - checkout: none
    - bash: ls -lRH
      displayName: Show local files
    - task: NexusIqPipelineTask@1
      displayName: Nexus IQ Scan
      continueOnError: true
      inputs:
        nexusIqService: 'Nexus Iq'
        applicationId: <your-application-name>.$(Build.Repository.Name)
        stage: 'Build'
        scanTargets: '$(System.DefaultWorkingDirectory)/**/*.*'
```
