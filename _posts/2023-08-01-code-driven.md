---
layout: post
title: "Building Distributed Architectures: Code driven"
---

*“I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it.”*
― Bill Gates

Nearly my entire professional life I have worked in small teams that built great (read big) products. 
The only way to efficiently do this is to automate as much as possible everything in the development process.
This is even more important for distributed architectures, where after a short amount of time you end up with a lot of 
moving parts that constantly need to support bug fixes, upgrades, or be replaced altogether. Typically I and my team have 
different environments in which our artifacts pass through until they reach the production environments. As a general rule
of thumb we have at least:
1. Dev environment - for current development work. This is an unstable environment, where things can be broken for a certain amount of time, but it is generally advisable to keep the disruptions as short as possible to prevent other team members from being blocked in their work. The development environment is linked to the CI/CD pipeline and any change that gets pushed to the git repository will trigger a build and a complete CI/CD workflow. This way we can always get a clear insight into the progress, and any errors that might show up and we can also check the interaction with the rest of the distributed environment.
2. Staging environment - this is a pre-release environment. As a feature is finished and the acceptance criteria are met, it will be deployed to the staging environment. This is also done through a CI pipeline, but it is manually triggered. We also keep a manual list of all components and their versions so that we can lock a certain component to a version when updating the staging environment. The staging environment is the one used by the QA team to test and only after the QA gives the green light, the changes will be pushed further to the production environment(s). Even if the version picking and deployment are manual processes, the rest of the deployment steps are fully automated.
3. Production environment(s) - these are the actual environments that are available to customers. Production environments are handled like precious pieces of art. Any change to the production must be planned, downtime must be minimized (or not exist at all) and the uptime must be maximized. Just like the staging environment, the production deployment is managed also through a CI pipeline, but the versioning and the triggering is a manual step.  This ensures full control over the process and at the same time, it minimizes the chance of human error during the deployment phase.

In our specific case, we have more than one production environment. Our product runs in a cloud environment (Azure) but at the same time, we have customers that want to host it in their own private clouds. This means that there can be differences between the platforms and we need to keep track of these differences.

To achieve these objectives, we use a stack that allows us to clearly track the infrastructure, and its evolution, have everything automated and at the same time have full control over everything:
1. [Microsoft Azure DevOps](https://azure.microsoft.com/en-us/products/devops/?nav=min)
2. [Microsoft Azure](https://azure.microsoft.com)
3. [HashiCorp Terraform](https://www.terraform.io/)
4. [Helm](https://helm.sh/)

1. Azure DevOps
Azure DevOps is the central product for all our code-related activities. We use Git repositories for our projects, Azure pipelines and agents for the CI/CD pipelines, and Azure Artifacts for any internal packages (as well as mirroring public packages). Each of our service projects (more on the project types in a later post) contains an azure pipeline YAML file that describes the steps needed to build the project. We have a shared project that contains the specific pipeline steps and the projects just reference these templates. This makes it easy to apply global fixes or changes without having to edit each project individually
2. 
