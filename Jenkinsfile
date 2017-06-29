#!/usr/bin/groovy

// Load shared library
@Library('c2c-pipeline-library') import static com.camptocamp.utils.*

dockerBuild {

    stage('syncronize geocat') {
        checkout scm

        withCredentials([usernamePassword(
            credentialsId: 'c2c-sig-ci-token',
            usernameVariable: 'GIT_USERNAME',
            passwordVariable: 'GIT_PASSWORD'
        )]) {
            sh("""
                rm -rf geocat.git
                git clone --mirror https://github.com/geoadmin/geocat.git
                cp -f gitconfig geocat.git/config
                cd geocat.git
                git config remote.c2c.url https://\$GIT_USERNAME:\$GIT_PASSWORD@github.com/camptocamp/geocat.git
                git remote update geoadmin
                git push c2c || true
            """)
        }
    }

    stage('set cron trigger every 4 hours') {
        checkout scm
        // set triiger every 2 hours
        setCronTrigger('H */2 * * *')
    }
}

