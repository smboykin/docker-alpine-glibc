// Jenkinsfile to use for ARCH workflow
node {
    def targetRegistry = env.DOCKER_STAGING_REGISTRY ?: 'docker-staging-local.chip-repo.childrens.harvard.edu'
    def imageName = env.ALPINE_GLIBC_IMAGE_NAME ?: 'ihlchip/alpine-glibc'
    def baseImageName = env.ALPINE_IMAGE ?: 'alpine'
    def image
    def imageMajorVersion = env.ALPINE_VERSION ?: '3.7'
    stage('Clone repository') {
        checkout scm
    }
    stage('Build and tag image') {
        // Use the provided build script
        sh "docker build -t ${imageName} --build-arg BASE_IMAGE=${baseImageName} --build-arg BUILD_VERSION=${imageMajorVersion} ."
        image = docker.image(imageName);

    }
    stage('Push image') {
        docker.withRegistry("https://${targetRegistry}", 'jenkins-chip-repo') {
            image.push()
            image.push("${imageMajorVersion}")
        }
    }
}