<div align="center">
  <h1>🐳🚢 Docker-Kubernetes-101</h1>

  <a href="">![Workflow](https://img.shields.io/github/workflow/status/jayantkatia/docker-kubernetes-101/Build%20and%20Publish%20Container%20Image?style=for-the-badge)</a>
  <a href="">![License](https://img.shields.io/github/license/jayantkatia/docker-kubernetes-101?style=for-the-badge)</a>
  <a href="">![Issues](https://img.shields.io/github/issues/jayantkatia/docker-kubernetes-101?style=for-the-badge)</a><br/>
  
  Learnings and approach for deployments using Docker and Kubernetes.  
</div>

## ✍️ Approach
### ✔️ Fork sub-directory from kubernetes/examples
To fork [kubernetes/examples/guestbook-go](https://github.com/kubernetes/examples/tree/master/guestbook-go) directory,
```sh
git clone https://github.com/kubernetes/examples.git
cd examples
git subtree split --prefix=guestbook-go -b main
git checkout main
# create a GitHub repo
git remote set-url origin YOUR_NEW_GIT_LINK
git fetch -pa
git push -u origin main
# clone the new repo
cd ..
git clone YOUR_NEW_GIT_LINK
cd YOUR_NEW_REPO
```

> Since the forked repo had [Apache License 2.0](https://github.com/kubernetes/examples/blob/master/LICENSE), we persevere the [LICENSE](https://github.com/jayantkatia/actions-for-docker/blob/main/LICENSE)

### ✔️ Prevent merging anything in main branch without review
GitHub has some granular and configurable settings to [enable branch protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule),
```
Settings:
    - [x] Require a pull request before merging
    - [x] Require approvals: 1 (testing purpose, do increase if required) 
    - [x] Dismiss stale pull request approvals when new commits are pushed
    - [x] Require review from Code Owners (may include CODEOWNERS file)
    - [ ] Do not allow bypassing the above settings (testing purpose)
```

### ✔️ Write GitHub action workflow 
```
Requirements:
    - Create a container image
    - Push that image to DockerHub
    - Build container image only when one of the below conditions is true,
        - When PR get merged in main/master branch from any other branch
        - When commit message contains `BUILD_CONTAINER_IMAGE` string
```
1. To build a container image use [docker's build-and-push GitHub action](https://github.com/marketplace/actions/build-and-push-docker-images)
2. Create a token on DockerHub and add credentials to repository secrets. Refer [DockerHub Docs](https://docs.docker.com/docker-hub/access-tokens/)
3. Add conditionals to action workflow
```yaml
...
on:
  push:
    branches: [main]
  pull_request:
    types:
      - closed
    branches:
      - main

jobs:
  build-publish-container-image:
    if: github.event.pull_request.merged == true || contains(github.event.head_commit.message, 'BUILD_CONTAINER_IMAGE')
```

### ✔️ Deploy container image to Kubernetes cluster
#### 🏡 Local Kubernetes Cluster using ```minikube```
1. Install and run ```minikube```, Refer [Gettting started with minikube](https://minikube.sigs.k8s.io/docs/start/).
    1. (Optional, Fedora specific steps) Prefer virtualization over containerization, due to known issues with btrfs and systemd.
     ```sh
     sudo dnf install @virtualization
     sudo systemctl start libvirtd
     sudo systemctl enable libvirtd
     
     # start minikube
     minikube start --driver kvm2
     ```
     
#### 🚂 Deployment
1. Make sure that your image path is correct and pointing to your DockerHub container image in ```guestbook-controller.json```,
```yaml
 ...
 "spec":{
            "containers":[
               {
                  "name":"guestbook",
                  "image":"jayantkatia/actions-for-docker:latest",
                  ...
               }
             ]
        }
  ...
```

2. Run,
```sh
kubectl apply -f redis-master-controller.json
kubectl apply -f redis-master-service.json
kubectl apply -f redis-replica-controller.json
kubectl apply -f redis-replica-service.json
kubectl apply -f guestbook-controller.json
kubectl apply -f guestbook-service.json
```
3. Run ```minikube tunnel``` to allocate an external IP to LoadBalancer.

#### 🎉 Result
![Screenshot from 2022-09-29 22-12-01](https://user-images.githubusercontent.com/56118625/193090218-643a3627-e389-43d1-a808-5b93cb0b77c2.png)
![Screenshot from 2022-09-29 22-18-45](https://user-images.githubusercontent.com/56118625/193091071-e12b254f-0d27-4590-9c9a-da797bfb8d70.png)
![Screenshot from 2022-09-29 22-22-25](https://user-images.githubusercontent.com/56118625/193091711-7cead1e3-be57-4de9-b79f-7d8b30681559.png)


## ✨ Contributing
Yes, please! Feel free to contribute, raise issues and recommend best practices.

A few resources to get you started:
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Documentation](https://docs.docker.com/)
- [MiniKube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes and MiniKube Refresher](https://www.youtube.com/watch?v=s_o8dwzRlu4)

<!-- Comment to change code and test workflow: 6 -->
