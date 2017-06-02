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
            echo 'protocol=https\nhost=github.com/camptocamp/geocat.git\nusername=${GIT_USERNAME}\npassword=${GIT_PASSWORD}\n\n' | git credential approve
            if \$(git submodule | grep geocat -n -qv);then
                git submodule add https://github.com/geoadmin/geocat.git geocat
            fi
            git submodule update --init
            cd geocat
            git fetch
            if \$(git remote -v | grep c2c -n -qv);then
                git remote add c2c https://github.com/camptocamp/geocat.git
            fi
            git push --mirror c2c
        """)
    }
}

