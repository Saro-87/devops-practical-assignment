# DevOps Practical Assignment

**Author:** G. Saravanan\
**GitHub Repository:**
https://github.com/Saro-87/devops-practical-assignment\
**AWS Region Used:** `ap-south-1`

## 1. Project Overview

This project demonstrates a practical DevOps workflow for a
containerized Flask application. It covers application health checks,
Docker image creation, CI automation with GitHub Actions, Kubernetes
deployment, static website hosting through Amazon S3 and CloudFront, and
an AWS high-availability/scaling architecture using an Application Load
Balancer (ALB), Auto Scaling Group (ASG), and a target-tracking CPU
scaling policy.

The AWS resources created for the practical exercise were removed after
testing to avoid ongoing charges. Therefore, the AWS-hosted website and
ALB endpoints are not expected to be available now. The source code and
GitHub repository are preserved.

## 2. Objectives

-   Build and test a small web application.
-   Containerize the application with Docker.
-   Automate validation/build steps using GitHub Actions.
-   Deploy multiple application replicas to Kubernetes.
-   Host static website content in Amazon S3 and distribute it through
    CloudFront.
-   Configure an AWS ALB, target group, launch template, Auto Scaling
    Group, and CPU-based scaling policy.
-   Validate application health and review AWS resources for cost
    control.

## 3. Technology Stack

  Area                          Technology
  ----------------------------- ---------------------------------------------------
  Application                   Python, Flask
  Containerization              Docker
  Source control                Git, GitHub
  CI/CD                         GitHub Actions
  Container registry            Amazon Elastic Container Registry (ECR)
  Container orchestration       Kubernetes, Minikube
  Static website                Amazon S3, Amazon CloudFront
  Load balancing                Application Load Balancer (ALB)
  Scaling                       EC2 Auto Scaling Group and target-tracking policy
  Cloud platform                Amazon Web Services (AWS)
  CLI / operating environment   AWS CLI, Windows PowerShell

## 4. Application Endpoints

The Flask application exposes:

-   `/` --- returns a JSON response identifying the application and
    reporting its status.
-   `/health` --- returns a health status used for basic verification.

Example root response:

``` json
{
  "application": "DevOps Practical Assignment",
  "status": "healthy"
}
```

Example health response:

``` json
{
  "status": "healthy"
}
```

The application was tested locally and through the Kubernetes service
during the exercise.

## 5. High-Level Architecture

``` text
Developer
   |
   v
Git repository on GitHub
   |
   v
GitHub Actions workflow
   |-- Run checks
   |-- Build Docker image
   |-- Push image to Amazon ECR (workflow configuration)
   |
   +--------------------------+
   |                          |
   v                          v
Kubernetes / Minikube      AWS web architecture
   |                          |
   v                          +--> Application Load Balancer
Application replicas          |          |
   |                          |          v
NodePort Service              |     Target Group
                              |          |
                              |          v
                              |     EC2 Auto Scaling Group
                              |     (CPU target: 70%)
                              |
Static website files --> S3 --> CloudFront
```

*Note: This diagram shows the intended project architecture. The AWS
resources were deleted after validation to control costs.*

## 6. CI/CD Workflow

The GitHub Actions workflow is stored under:

``` text
.github/workflows/ci.yml
```

The workflow was pushed to the GitHub repository and completed
successfully during testing. It was used to validate the pipeline and
build the Docker image. The workflow includes the ECR image publishing
stage as configured in the repository.

To view the workflow history, open the repository's **Actions** tab:

https://github.com/Saro-87/devops-practical-assignment/actions

**Important:** Since the AWS ECR repository and related AWS resources
were deleted during cost cleanup, a future workflow run that attempts to
push to ECR will require the repository to be recreated and the required
GitHub Actions secrets/permissions to be configured.

## 7. Docker

The application was packaged as a Docker image and run in a local
container for validation.

Typical commands:

``` powershell
docker build -t devops-practical-app:1.0 .
docker run --name devops-practical-container -p 5000:5000 devops-practical-app:1.0
```

Open the application locally at:

-   http://localhost:5000/
-   http://localhost:5000/health

If a container with the same name already exists, remove or rename that
container before running the command again. Confirm the port and
application start command against the repository's Dockerfile before
rebuilding.

## 8. Kubernetes Deployment

The application was deployed to a local Minikube Kubernetes cluster with
two replicas and a NodePort service.

The deployment was tested during the exercise. This local deployment is
separate from the AWS resources that were removed during cleanup.

Useful inspection commands:

``` powershell
kubectl get deployments
kubectl get pods
kubectl get services
kubectl describe deployment
```

Use the actual deployment and service names defined in the Kubernetes
manifests in the repository.

## 9. AWS Static Website: S3 and CloudFront

The static website was configured using an Amazon S3 bucket and a
CloudFront distribution. An Origin Access Control (OAC) was used for
CloudFront access to the S3 origin, and a CloudFront invalidation was
completed during testing.

The following project resources were removed during cleanup:

-   S3 static website bucket and its website object
-   CloudFront distribution
-   CloudFront Origin Access Control

Consequently, the CloudFront URL is no longer expected to serve the
project website.

## 10. AWS Load Balancing and Auto Scaling

The AWS architecture included:

-   EC2 launch template
-   Application Load Balancer
-   Target group
-   Auto Scaling Group
-   Target-tracking scaling policy targeting **70% average CPU
    utilization**
-   VPC and supporting networking resources

The load balancer and Auto Scaling architecture were created and tested.
Resources were subsequently removed to prevent ongoing AWS costs. The
live ALB endpoint is therefore no longer available.

## 11. Validation and Results

  -----------------------------------------------------------------------
  Test                                Result
  ----------------------------------- -----------------------------------
  Flask root endpoint                 Passed during testing

  Flask health endpoint               Passed during testing

  Docker image build and container    Completed during testing
  test                                

  GitHub Actions workflow             Successful run recorded

  Kubernetes application              Tested during the exercise
  replicas/service                    

  S3 and CloudFront website           Tested before cleanup

  ALB / target group / Auto Scaling   Created and tested during the
  configuration                       exercise

  AWS cleanup                         Assignment resources deleted; final
                                      account-wide billing review is
                                      still recommended
  -----------------------------------------------------------------------

Results above describe the tests performed during the assignment. They
do not imply that the deleted AWS endpoints remain online.

## 12. Repository Structure

The repository contains the application and supporting DevOps files.
Refer to the actual repository tree for the definitive list of files and
manifests.

Expected key components include:

``` text
devops-practical-assignment/
├── .github/
│   └── workflows/
│       └── ci.yml
├── Dockerfile
├── application source files
├── Kubernetes manifests
└── README.md
```

*The names and paths above are illustrative where noted; use the
committed files in the repository as the source of truth.*

## 13. Evidence for Submission

Recommended evidence screenshots:

1.  GitHub repository page showing the project files.
2.  GitHub Actions run showing a successful workflow.
3.  Docker image build and application response.
4.  Kubernetes pods and service output.
5.  S3/CloudFront website working before cleanup, if the screenshot was
    captured.
6.  ALB target health or application response before cleanup, if
    captured.
7.  AWS cleanup checks showing deleted resources.

Only include screenshots that were actually captured. The AWS-hosted
endpoints are no longer expected to be active because resources were
deleted for cost control.

## 14. Conclusion

This practical assignment demonstrates a container-based application
workflow using GitHub, GitHub Actions, Docker, Kubernetes, and AWS
services for static content delivery, load balancing, and scaling. The
workflow was tested, and the AWS resources used for the exercise were
removed afterward to reduce potential charges. The GitHub repository
remains the primary source for reviewing the implementation.

**Repository:** https://github.com/Saro-87/devops-practical-assignment
