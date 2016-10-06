pipeline {
    environment {
        FOO = "BAR"
    }

    agent label:''

    stages {
        stage("foo") {
            sh 'echo "FOO is $FOO"'
        }
    }
}
