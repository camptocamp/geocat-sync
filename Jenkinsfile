#!/usr/bin/groovy

// Load shared library
@Library('c2c-pipeline-library') import static com.camptocamp.utils.*

dockerBuild {
    checkout scm
    setCronTrigger('*/10 * * * *')
    stage 'syncronize geocat'
    sh 'git remote add geoadmin https://github.com/geoadmin/geocat.git'
    sh 'git fetch geoadmin'
    withCredentials([usernamePassword(credentialsId: 'c2c-sig-ci-token' usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
        // configure the git credentials, these are cached in RAM for several minutes to use
        // this is required until https://issues.jenkins-ci.org/browse/JENKINS-28335 is resolved upstream
        sh "echo 'protocol=https\nhost=github.com/camptocamp/geocat.git\nusername=${GIT_USERNAME}\npassword=${GIT_PASSWORD}\n\n' | git credential approve "
        sh "git push --mirror"
    }
}

