# Actions for Docker

## Requirements
- [x] Create a container image using Github Action workflow and push that image to DockerHub.
- [x] Deploy container image built in local/any kubernetes cluster.
- [x] Prevent merging anything in main branch without review.
- [x] Build container image only when one of the below conditions is true:
    - When PR get merged in main/master branch from any other branch.
    - When commit message contains `BUILD_CONTAINER_IMAGE` string

<!-- Comment to change and test workflow: 2 -->