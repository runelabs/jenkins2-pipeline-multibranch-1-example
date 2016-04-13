#!groovy

stage 'Source'
    node {
        echo 'Retrieving source...'
        checkout scm
    }

stage 'Setup'
    node {
        echo 'Setting environment...'
    }

stage 'Dependencies'
    node {
        echo 'Retrieving dependencies...'
        bat 'mkdir -p deps'
        bat 'bash -c \'echo "some-dependency-content-perhaps-binary-somedep-1220" > deps/somedep-1.22.0.dep\''
        bat 'bash -c \'echo "some-dependency-content-perhaps-binary-otherdep-421132" > deps/otherdep-4.2.113-2.dep\''
        bat 'bash -c \'date +%s ; sleep "$(date +%s | cut -c-2)" ; date +%s\''
        step([$class: 'ArtifactArchiver', artifacts: 'deps/**/*', fingerprint: true])
    }

stage 'Build'
    node {
        echo 'Building...'
        bat 'bash -c \'for i in 1 2 3 4 ; do echo "Compile file $i"; sleep $i ; done\''
    }

stage 'Unit tests'
    node {
        echo 'Performing unit tests...'
    }

stage 'Pack'
    node {
        echo 'Packaging...'
    }

stage 'Extended tests'
    node {
        echo 'Performing extended testing...'
    }

stage 'Artifact'
    node {
        echo 'Storing build artifacts...'
    }
