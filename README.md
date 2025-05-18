**CI/CD Pipelining Demo**

A DevOps project demonstrating a full-featured CI/CD pipeline using Jenkins and Kubernetes.

## Table of Contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [Repository Structure](#repository-structure)
* [Jenkins Pipeline](#jenkins-pipeline)
* [Kubernetes Manifests](#kubernetes-manifests)
* [Getting Started](#getting-started)
* [Usage](#usage)
* [Contributing](#contributing)
* [License](#license)

---

## Overview

This repository illustrates how to set up a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a sample application. It uses:

* **Jenkins** as the CI/CD orchestrator.
* **Docker** to build and package the application into an image.
* **Kubernetes** for container orchestration and deployment.

The pipeline includes stages for building, testing, pushing the Docker image, and deploying it to a Kubernetes cluster.

## Prerequisites

Before you begin, ensure you have:

* A running **Jenkins** server (version 2.0+).
* **Docker** installed on the Jenkins agent.
* A **Docker registry** (Docker Hub, ECR, GCR, etc.).
* A **Kubernetes** cluster (local or cloud).
* **kubectl** configured to communicate with your cluster.

## Repository Structure

```
├── Jenkinsfile         # Pipeline definition
├── k8s/
│   ├── deployment.yaml # Kubernetes Deployment manifest
│   └── service.yaml    # Kubernetes Service manifest
└── app/
    ├── Dockerfile      # Builds the sample application
    └── src/            # Application source code
```

* `Jenkinsfile`: Defines the pipeline stages.
* `app/Dockerfile`: Instructions to containerize the sample app.
* `k8s/`: Contains manifests to deploy the container on Kubernetes.

## Jenkins Pipeline

The **Jenkinsfile** covers the following stages:

1. **Checkout**: Pulls the latest code from the repository.
2. **Build**: Builds the Docker image using the `app/Dockerfile`.
3. **Unit Test**: (Optional) Runs application unit tests.
4. **Push**: Tags and pushes the image to the configured Docker registry.
5. **Deploy**: Applies the Kubernetes manifests (`k8s/deployment.yaml` and `k8s/service.yaml`) to the target cluster.

You can customize agent labels, environment variables, and credentials in the pipeline.

## Kubernetes Manifests

### Deployment

* Defines a **Deployment** named `cicd-demo`.
* Uses the Docker image built in the pipeline.
* Configures replicas, container ports, and resource requests/limits.

### Service

* Defines a **Service** of type `ClusterIP` named `cicd-demo-service`.
* Exposes the Deployment on a specified port.

Feel free to modify replicas, update strategies, and service types (e.g., `LoadBalancer`, `NodePort`).

## Getting Started

1. **Clone** this repository:

   ```bash
   git clone https://github.com/pahalsrivastava/cicd-pipelining.git
   cd cicd-pipelining
   ```

2. **Configure Jenkins**:

   * Create a new Pipeline job.
   * Point it to this repository URL.
   * Provide credentials for Docker registry and Kubernetes (if needed).

3. **Run the Pipeline**:

   * Trigger a build in Jenkins.
   * Monitor console output for build, push, and deploy steps.

4. **Verify Deployment**:

   ```bash
   kubectl get pods
   kubectl get svc cicd-demo-service
   ```

Access the application via the Service endpoint.

## Usage

* **Iterate**: Commit changes to the `app/src` directory. Jenkins will rebuild and redeploy automatically.
* **Customize**: Add test stages, linting, or security scans in the Jenkinsfile.
* **Scale**: Adjust replica counts in `k8s/deployment.yaml`.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with enhancements or bug fixes.

## License

This project is licensed under the [MIT License](LICENSE).
