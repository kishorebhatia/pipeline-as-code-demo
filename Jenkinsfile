#!groovy

stage 'Dev'
node {
    checkout scm
    mvn 'clean package'
    dir('target') {stash name: 'war', includes: 'x.war'}
}




def mvn(args) {
    sh "${tool 'Maven 3.x'}/bin/mvn ${args}"
}

