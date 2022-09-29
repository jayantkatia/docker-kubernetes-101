# 🚀 Docker-Kubernetes-101
Learnings and approach for deployments using Docker and Kubernetes.  

## ✍️ Approach
### ✔️ Fork sub-directory from kubernetes/examples
### ✔️ Create a container image using Github Action workflow and push that image to DockerHub
### ✔️ Prevent merging anything in main branch without review
### ✔️ Build container image on basis of conditional checks
```
Conditions:
    - When PR get merged in main/master branch from any other branch
    - When commit message contains `BUILD_CONTAINER_IMAGE` string
```

### ✔️ Deploy container image built in local/any kubernetes cluster

## ✨ Contributing
Yes, please! Feel free to contribute, raise issues and recommend best practices.

A few resources to get you started:
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Documentation](https://docs.docker.com/)
- [MiniKube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes and MiniKube Refresher](https://www.youtube.com/watch?v=s_o8dwzRlu4)

<!-- Comment to change code and test workflow: 6 -->
