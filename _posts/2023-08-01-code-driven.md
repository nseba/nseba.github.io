---
layout: post
title: "Building Distributed Architectures: The Code-Driven Approach"
---

*“I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it.”*
― Bill Gates

Greetings, fellow developers and tech enthusiasts! Today, I want to share with you a valuable approach that has been the backbone of our development process, helping us build remarkable and large-scale products with minimal effort. Welcome to the world of code-driven distributed architectures!

![_config.yml]({{ site.baseurl }}/images/2023-08-01/code.jpg)

> Before going further, this article is part of a series covering how to build an effective distributed architecture. If you haven't already, I recommend reading the previous articles to get up to speed:
> 1. [Building Distributed Architectures: Creating a Blueprint for Success](/building-distributed-architectures-creating-a-blueprint-for-success/)
> 2. [Goals](/goals/)

## The Power of Automation

Throughout my professional journey, I've been fortunate to work with fantastic small teams on ambitious projects. To achieve our goals efficiently, we knew automation was the key. And when it comes to distributed architectures, the complexity demands an even greater emphasis on streamlining the development process.

In this blog post, I'll delve into our strategy, using standard tools that empower us to automate and orchestrate with ease.

### Environments: The Crucial Building Blocks

In our development workflow, we have various environments, each serving its unique purpose. These environments form the foundation for our smooth progression toward production:

1. **Dev Environment**: This is where the magic begins. A playground for current development work, the dev environment allows us to experiment and iterate rapidly. Though it may be a bit unstable, we ensure minimal disruptions to keep our team's productivity at its peak. Continuous Integration and Continuous Deployment (CI/CD) pipelines are the backbone, and any changes pushed to the Git repository trigger a complete CI/CD workflow, giving us clear insights into the progress and interaction with the distributed environment.

2. **Staging Environment**: Here comes the pre-release phase. When a feature is ready, it's deployed to the staging environment, which is manually triggered. We meticulously maintain a list of components and their versions, locking certain components to specific versions during staging updates. Our Quality Assurance (QA) team rigorously tests the changes, and only after they give the green light, do the changes proceed to the production environment(s). Although version picking and deployment triggering are manual, the rest of the deployment steps are fully automated.

3. **Production Environment(s)**: The grand finale! These are the environments accessible to our valued customers. Production environments are treated with utmost care, planned changes, minimized downtime, and maximized uptime. Deployments to production are managed through CI pipelines, with versioning and triggering being manual steps. This approach provides us with complete control, minimizing human errors during deployment.

## Empowering Tools and Procedures

We equip ourselves with a powerful tech stack that ensures full control, automation, and transparency throughout the development lifecycle. Here are the tools that make our dreams come true:

1. **[Microsoft Azure DevOps](https://azure.microsoft.com/en-us/products/devops/?nav=min)**: The heart of our code-related activities resides in Azure DevOps. Git repositories, Azure pipelines, and Azure Artifacts form the core of our CI/CD workflow. Each service project contains an Azure pipeline YAML file describing the build steps. We leverage shared project templates for easy global fixes, saving us from tedious project-by-project edits.

2. **[Microsoft Azure](https://azure.microsoft.com)**: Our main platform, Azure, provides a robust foundation. Despite the need to support other environments, we ensure minimal dependencies on Azure-specific functionalities. Azure securely stores our Terraform state, allowing multiple developers to work simultaneously without collision risks. Secrets generated during provisioning phases are safeguarded in an Azure Key Vault, ensuring smooth operation even during challenging times.

3. **[HashiCorp Terraform](https://www.terraform.io/)**: Terraform empowers us to describe cloud infrastructure in a declarative, coded manner. Its flexibility allows different approaches for various target platforms. We use Terraform for setting up infrastructure, be it DNS, managed services or Kubernetes. Additionally, we use Terraform to provision Kubernetes services based on 3rd party Helm charts, along with Kubernetes resources that are not part of any chart (secrets, config maps).

4. **[Helm](https://helm.sh/)**: Helm is our trusty companion for service deployments. Helm charts enable atomic state management, making it easy to apply changes to services as a whole. Rollbacks to previous versions become effortless, saving us from sleepless nights during troubleshooting. In the dev environment, in some scnearios we must manually make changes using kubectl or k9s. Once validated, the changes are incorporated into the corresponding chart.

## Keeping things together

In order to achieve maximum flexibility and reusability, the entire infrastructure is set up based on Terraform. The approach to this is similar as any other programming approach, with an accent on modularity and reusability.

Each environment that we manage is set up as an independent module, with other modules providing reusable building blocks. Having the environments as separate modules allows us to tweak the installation and alter it based on the specifics. For example, environments can be identical (of course with independent DNS records and services), or they can be customized with specific services (for example enterprise-dedicated clusters can be set-up with customer-specific integration services). 

Although the environments are described individually, the building blocks of the environments are also available as modules, so that it is an easy job to reuse the configurations and apply changes globally, if needed.

Below is a high-level diagram of such an environment module structure:

![_config.yml]({{ site.baseurl }}/images/2023-08-01/terraform-structure.jpg)

As mentioned previously, we backup all secrets into a secured Azure Key Vault, while everything else is set up in the terraform files and modules. Since our main runtime platform is Kubernetes, any 3rd party chart that is required by our product is also installed through the Terraform helm provider. Also, global secrets and config maps are also created through Terraform. This allows us to generate secrets on the first deployment and then use them, without human intervention (which reduces the exposed security surface).


## Conclusion: Unleashing the Full Potential

Taking a code-driven approach to infrastructure from the very beginning unleashes a world of possibilities. With increased speed, replicability, and transparency, we minimize human-induced errors, allowing us to meet our goals effectively. Automated or semi-automated environments help us identify issues swiftly, allowing rapid cloud environment setup for various stages or multiple production clouds.

With all infrastructure described in Terraform files stored in Git repositories, we gain a transparent history of changes and the ability to roll back with ease. Safeguarding critical data like Terraform files and secrets is paramount, ensuring quick disaster recovery when needed.

Remember, CI/CD policies can be organization-specific, but a balanced approach that updates the dev environment quickly while ensuring a controlled process for staging and production environments works wonders.

Embrace the power of code-driven architectures, and witness your development process soar to new heights!

Thank you for joining me on this exciting journey. Stay tuned for more insights and tech adventures in my future blog posts!

Happy coding!
