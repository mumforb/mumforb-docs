# IaC Pipeline Scanners

As [Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) (IaC) increases in complexity and scale, and provisions assets with sensitive material, it is critically important to have scanners that look for a wide variety of vulnerabilities at every stage of development and requisition. It isn't enough to scan at the time of writing the IaC code, or once the assets have been deployed to AWS. As such, we have developed strategies to scan the IaC locally, in the Gitlab Pipeline(s), and in production.

!!! note
    These scans can be added to Gitlab's Compliance Pipelines, which means that if your project is covered by the Compliance Framework, these scanners will run automatically.

## Custom IaC Scanners

### Gitlab-provided Scanning

Gitlab has a suite of built-in, freely available SAST scanners, [one of which is specifically designed for IaC.](https://docs.gitlab.com/ee/user/application_security/iac_scanning/) It is activated in a Gitlab CI/CD pipeline by simply adding the following line to the `Include` block of a `.gitlab-ci.yml`:

```
include:
  - template: Security/SAST-IaC.gitlab-ci.yml
```

!!! note
    There is Gitlab documentation on the other SAST scanners available.

The underlying scanning protocol is provided by [KICS](https://kics.io/), which is a thorough scanning platform, but does not adhere to any specification beyond its own.

### Additional Scanning

Specifically for IaC, there are a variety of addition scanning platforms beyond KICS, each of which has its own pros and cons. These include (but are not necessarily limited to):

- [Snyk](https://app.snyk.io/group/841e58d2-f5b8-418a-a00c-ca028222ee6f/reports/)
- [Checkov](https://www.checkov.io/)
- [TFLint](https://github.com/terraform-linters/tflint)
- [Terrascan](https://runterrascan.io/)
- [TFSec](https://tfsec.dev)
- [Terraform Compliance](https://terraform-compliance.com/)

At this time, we are utilizing several of these solutions to ensure that there are no gaps in coverage, and we put our IaC in the very best position to shift left and thusly reduce the complexity of problems that appear once infrastructure has been deployed.

In order to activate one of these scans in your pipeline, similarly to the built-in Gitlab scan, simply include the reference to the template:

```yaml
include:
  - template: Security/SAST-IaC.latest.gitlab-ci.yml
  - project: "devops/helpers"
    ref: "main"
    file: "ci-templates/sast/iac/checkov.yml"
  - project: "devops/helpers"
    ref: "main"
    file: "ci-templates/sast/iac/tflint.yml"
  - project: "devops/helpers"
    ref: "main"
    file: "ci-templates/sast/iac/tfsec.yml"
  ```

This example will conduct all the steps consistent with these scanners.

## Custom Scanner Converters

The custom scanners reside in their own repository, and are fairly simple scripts written in Typescript (compiled to JavaScript) and designed to be run in a stage in Gitlab CI/CD pipeline. Because they are stand-alone JavaScript server-side scripts, they are designed to use an environment that has NodeJS installed. 

The reason to convert the scan's output is so that it will appear in Gitlab's built-in Security Dashboard, which is enabled as part of our service agreement. This way the default output (if enabled) and any custom output will appear in the same location, which will make tracking and remediation more streamlined and traceable. Currently there are scripts for Checkov and Snyk.