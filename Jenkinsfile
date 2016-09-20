def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}

node {
    // Checkout source code from Git
    stage 'Checkout'
    checkout scm

    // Build Docker image
    stage 'Build'
    sh "docker build -t dcotestrob2/test1:${gitCommit()} ."

    // Log in and push image to GitLab
    stage 'Publish'
    withCredentials(
        [[
            $class: 'UsernamePasswordMultiBinding',
            credentialsId: '50e19c55-4843-40f7-9b30-9b5688254d20',
            passwordVariable: 'DOCKERHUB_PASSWORD',
            usernameVariable: 'DOCKERHUB_USERNAME'
        ]]
    ) {
        sh "docker login -u ${env.DOCKERHUB_USERNAME} -p ${env.DOCKERHUB_PASSWORD} -e demo@mesosphere.com"
        sh "docker push dcorobtet2/test1:${gitCommit()}"
    }
}

