#!groovy



properties([
    [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '1', daysToKeepStr: '', numToKeepStr: '']]
  ])

node("windows") {
    wrap([$class: 'TimestamperBuildWrapper']) {
      def startTime = System.currentTimeMillis()
      def wsDir = getWorkspace(startTime)
      wsDir = 'xam'
      envVersion = '1.0.0.272'
      envPlatform = 'ARM'
      envConfig = 'Debug'
      envOutput = 'output'
      envProject = 'Capture.UniWin/Capture.UniWin.WindowsPhone/Capture.UniWin.WindowsPhone.csproj'
      envMsbuild = '/c/Program\\ Files\\ \\(x86\\)/MSBuild/14.0/Bin/MSBuild.exe'
      envCmd = "$envMsbuild -p:Platform=$envPlatform -p:Configuration=$envConfig -p:OutputPath=$envOutput $envProject"
      ws (wsDir) {
        stage 'Workspace'

        echo "Using " + wsDir
        pwd()
        echo 'Cleaning...'
//        deleteDir()

        stage 'Source'

        echo 'Retrieving source...'
//        checkout scm
        sh ''' git remote -v '''
        sh ''' git branch -va '''
        sh ''' git status '''

        echo 'Retrieving submodules...'

        sh ''' git submodule init '''
        sh ''' git submodule sync '''
        sh ''' git submodule update '''




        stage 'Setup'
 
        echo 'Setting environment...'
        sh """ python set_package_version_number.py "$envVersion" """
//        sh '''#!bash
//            echo hei igjen fra bash
//        '''
//        sh '''#!python
//print("heisann fra python")
//        '''
 

        stage 'Dependencies'

        echo 'Retrieving dependencies...'
        sh ''' .nuget/NuGet.exe restore Capture.sln '''


        stage 'Fingerperinting'

        echo 'Fingerprinting dependencies...'
        //step([$class: 'ArtifactArchiver', artifacts: 'deps/**/*', fingerprint: true])
        step([$class: 'Fingerprinter', targets: 'packages/**/*'])
        step([$class: 'Fingerprinter', targets: './**/*.dll'])


        stage 'Build'

        echo 'Building...'
        timeout(time: 30, unit: 'SECONDS') {
          sh """ $envCmd """
        }

        echo 'Fingerprinting artifacts...'
        step([$class: 'Fingerprinter', targets: './**/*.appx'])
        step([$class: 'Fingerprinter', targets: './**/*.dll'])


        stage 'Unit tests'

          envTest1 = 'NetlifeLib/NetlifeLib.Common.UnitTest/NetlifeLib.Common.UnitTest.csproj'
          envTest2 = 'Capture.Core.UnitTest/Capture.Core.UnitTest.csproj'
          envTest3 = 'Capture.Model/Capture.Model.Impl.UnitTest/Capture.Model.Impl.UnitTest.csproj'

          sh """ $envMsbuild $envTest1 """
          sh """ $envMsbuild $envTest2 """
          sh """ $envMsbuild $envTest3 """


        echo 'Performing unit tests...'

          envTestCmd = 'packages/NUnit.Console.3.0.1/tools/nunit3-console.exe NetlifeLib/NetlifeLib.Common.UnitTest/bin/Debug/NetlifeLib.Common.UnitTest.dll Capture.Core.UnitTest/bin/Debug/Capture.Core.UnitTest.dll Capture.Model/Capture.Model.Impl.UnitTest/bin/Debug/Capture.Model.Impl.UnitTest.dll'
          sh """ $envTestCmd """

        echo 'Performing coverage...'
          sh """
            cd UnitTestCoverage
            python ./run_dotcover.py
          """


        stage 'Pack'

        echo 'Packaging...'


        stage 'Extended tests'

        echo 'Performing extended testing...'


        stage 'Artifact'

        echo 'Storing build artifacts...'

      }
    }
}


def getWorkspace(time) {
    pwd().replace("%2F", "_") + '-' + time
}
