// Jenkinsfile to use for ARCH workflow
node {
    def targetRegistry = env.DOCKER_STAGING_REGISTRY ?: 'docker-staging-local.chip-repo.childrens.harvard.edu'
    def imageName = env.ALPINE_GLIBC_IMAGE_NAME ?: 'ihlchip/alpine37-glibc'
    def baseImageName = env.S6_IMAGE ?: 'ihlchip/alpine37-s6'
    def image
    stage('Clone repository') {
        checkout scm
    }
    stage('Build and tag image') {
        // Use the provided build script
        sh "docker build -t ${imageName} --build-arg BASE_IMAGE=${baseImageName} ."
        image = docker.image(imageName);

    }
    stage('Push image') {
        docker.withRegistry("https://${targetRegistry}", 'jenkins-chip-repo') {
            image.push()
        }
    }
}