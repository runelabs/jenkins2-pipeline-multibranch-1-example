#!groovy



node {
    wrap([$class: 'TimestamperBuildWrapper']) {

        stage 'Clean'

        echo 'Cleaning workspace...'
        deleteDir()

        stage 'Source'

        echo 'Retrieving source...'
        checkout scm


        stage 'Setup'
 
        echo 'Setting environment...'
 

        stage 'Dependencies'

        echo 'Retrieving dependencies...'
        bat 'if not exist deps mkdir deps'
        bat 'echo some-dependency-content-perhaps-binary-somedep-1220 > deps/somedep-1.22.0.dep.txt'
        bat 'echo some-dependency-content-perhaps-binary-otherdep-421132 > deps/otherdep-4.2.113-2.dep.txt'
        bat 'date && sleep 10 && date'


        stage 'Fingerperinting'

        echo 'Fingerprinting dependencies...'
//        step([$class: 'ArtifactArchiver', artifacts: 'deps/**/*', fingerprint: true])


        stage 'Build'

        echo 'Building...'
//        bat 'bash -c "for i in 1 2 3 4 ; do echo Compile\\ file\\ $i ; sleep $i ; done"'


        stage 'Unit tests'

        echo 'Performing unit tests...'


        stage 'Pack'

        echo 'Packaging...'


        stage 'Extended tests'

        echo 'Performing extended testing...'


        stage 'Artifact'

        echo 'Storing build artifacts...'

    }
}