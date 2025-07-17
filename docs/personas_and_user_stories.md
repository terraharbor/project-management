# Personas and User Stories

This document outlines the personas and user stories for the TerraHarbor project.

## Personas

### Cloud Architect

Alex is a Cloud Architect for a small to mid-size company. He designs and implements cloud solutions for various internal projects.

He is an experienced Terraform user and certified in AWS and Azure. He is responsible for managing the infrastructure of multiple projects, ensuring that they are secure, scalable, and cost-effective.

He deploys and manages cloud infrastructure using Terraform following an Infrastructure as Code approach. For each project/platform, he keeps a separate Terraform code to manage the infrastructure. Each project has its own Terraform state file.

After storing the Terraform state files in a remote backend such as AWS S3, he would like to have a dedicated solution to manage the Terraform state files only, with features such as versioning, encryption, and access control.

### Project Administrator

Amanda is a Project Administrator in the same company as Alex.

She oversees multiple projects and coordinates between different teams.

She is responsible for attributing users to teams and to attribute teams to projects. She is also responsible for managing the tokens used by automation tools to access the Terraform state files. She would also like to control an encryption mechanism for the Terraform state files to ensure that they are secure at rest.

### TerraHarbor Administrator

Tim is a TerraHarbor Administrator for the same company.

He is responsible for deploying and maintaining the TerraHarbor application for its company, namely for users such as Alex and Amanda.

### TerraHarbor Developer

A TerraHarbor Developer is in fact one of us (Anthony, Badis, Fabrice, Gonçalo, Sven) who is developing the TerraHarbor application.

We are responsible for implementing the features and functionalities of the TerraHarbor application, ensuring that it meets the requirements of the personas and user stories defined in this document. As developers of the application, we also have some requirements for the application, such as being able to test it locally, deploy it easily, and have a good development experience.

## User Stories

### Application development

As a **TerraHarbor Developer**,

- I want to be able to develop the application locally, so that I can test it and implement new features easily;
- I want to be able to deploy the application easily, so that I can test it in a staging environment before deploying it to production;
- I want to have a good development experience, so that I can be productive and efficient in my work.

### Application deployment in production

As a **TerraHarbor Administrator**,

- I want to be able to deploy the TerraHarbor application in production, so that it is available for company users;
- I want the deployment process to be easy and automated;
- I want to avoid having to manage the dependencies manually;
- I want to be able to upgrade the application easily, with minimum downtime, so that I can benefit from the latest features and bug fixes;
- Ideally, I want to be able to deploy the application in a Kubernetes cluster, so that it is scalable and resilient.

### Application usage

As a **Cloud Architect**,

- I want to use Terraform state files stored in TerraHarbor as a remote backend, following the API specification used by Terraform;
- I want to be able to see multiple versions of a Terraform state file, compare them, and revert to a previous version if needed;
- I want to use Terraform state files stored in TerraHarbor as a remote backend, so that multiple users can collaborate on the same Terraform code;
- I want to be able to encrypt a Terraform state file, so that it is secure at rest;
- I want to be able to download and decrypt a Terraform state file manually in case TerraHarbor is unavailable, so that I can still run Terraform commands locally in case of an emergency;
- I want to be able to access the Terraform backend using a personal access token, so I can run Terraform commands locally without using the project token;
- I want to be able to see who is locking a Terraform state file and unlock it manually in case of errors if needed;
- I want to get a `terraform init` command to initialize the Terraform backend with the TerraHarbor application, so that I can easily start using it.

As a **Project Administrator**,

- I want to be able to create and manage teams, so that I can which users can access which Terraform state files;
- I want to be able to create projects and group Terraform state files by project, so that I can manage the access to the Terraform state files more easily;
- I want to be able to create technical users/tokens with access to the Terraform state files, so that I can automate the deployment of the infrastructure using CI/CD pipelines;
- I want to be able to manage the access control of the Terraform state files, so that I can ensure that only authorized users can access them;
- I want to see an audit log of all access to the Terraform state files, so that I can monitor for any unauthorized access.

As either a **Cloud Architect** or a **Project Administrator**,

- I want to easily login to the TerraHarbor application, ideally using a single sign-on (SSO) solution, so that I can access the application without having to remember multiple passwords;
- I want to be able to manage my personal access tokens, so that I can revoke them if needed;
- I want to be able to manage my personal settings, such as language and theme, so that I can customize the application to my preferences;
- I want to be able to see which teams I belong to and which projects I have access to, so that I can easily navigate the application and find the Terraform state files I need.
