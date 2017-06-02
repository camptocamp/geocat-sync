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
            if \$(git submodule | grep -qv geocat);then
                git submodule add https://github.com/geoadmin/geocat.git geocat
            fi
            git submodule update --init
            cd geocat
            git fetch
            git remote remove c2c
            git remote add c2c https://\$GIT_USERNAME:\$GIT_PASSWORD@github.com/camptocamp/geocat.git
            git push --mirror c2c
        """)
    }
}

