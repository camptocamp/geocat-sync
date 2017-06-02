#!/usr/bin/groovy

// Load shared library
@Library('c2c-pipeline-library') import static com.camptocamp.utils.*

dockerBuild {
    checkout scm
    setCronTrigger(*/10 * * * *')
    stage 'syncronize geocat'
    sh 'git remote add geoadmin https://github.com/geoadmin/geocat.git'
    sh 'git fetch geoadmin'
    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'c2c-sig-ci-token', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
        sh('git push --mirror https://${GIT_USERNAME}:${GIT_PASSWORD}@https://github.com/camptocamp/geocat.git')
    }
}

