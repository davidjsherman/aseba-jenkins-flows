pipeline {
    environment {
        FOO = "BAR"
    }

    agent label:"master"

    stages {
        stage("foo") {
            sh 'echo "FOO is $FOO"'
        }
    }
}
