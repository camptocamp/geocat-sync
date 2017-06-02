#!/usr/bin/groovy

// Load shared library
@Library('c2c-pipeline-library') import static com.camptocamp.utils.*

dockerBuild {
    checkout scm
    setCronTrigger('*/10 * * * *')
    stage 'syncronize geocat'

    withCredentials([usernamePassword(
        credentialsId: 'c2c-sig-ci-token',
        usernameVariable: 'GIT_USERNAME',
        passwordVariable: 'GIT_PASSWORD'
    )]) {
        sh("""
            git clone --mirror https://github.com/geoadmin/geocat.git
            cd geocat.git
            git branch -a
            git fetch --all
            git remote remove c2c
            git remote add c2c https://\$GIT_USERNAME:\$GIT_PASSWORD@github.com/camptocamp/geocat.git
            git push c2c --mirror
        """)
    }
}

