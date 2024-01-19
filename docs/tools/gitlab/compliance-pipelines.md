# Compliance Pipelines

Compliance Pipelines are Gitlab CI pipelines that run on specific projects that have been added to a [Compliance Framework](https://docs.gitlab.com/ee/user/project/settings/index.html#compliance-frameworks). They, in turn, combine with the project's pipelines to cover test, build and deploy tasks, along with other compliance-related tasks.

## What is a Compliance Framework

A Compliance Framework, in brief, is a way to apply consistent, centralized, and enterprise-guided tasks to a large set of CI Pipelines. By applying a Compliance Framework to an individual project, team leadership can be assured that defined quality and security tasks are being universally performed, with all the up-flowing reporting and insight that goes along with.

## What is a Compliance Pipeline

A Compliance Pipeline, specifically, is the actual set of commands that define CI/CD stages, and the resulting pipeline runs. These pipelines are defined in a `yml` file, located inside a Gitlab repository, with all the syntactical options available in a regular `.gitlab-ci.yml`. The thing to keep in mind here is that a compliance pipeline, once enabled, supersedes the regular `.gitlab-ci.yml` file, and it cannot be overwritten. In fact, the compliance pipeline technically needs to call the local `.gitlab-ci.yml` file, or it will not run at all.

Once enabled and configured, the compliance pipeline operates similarly to a regular CI file. Obviously it has different goals (you would not use a compliance pipeline for a build or promotion step, for example), but otherwise it is the same kind of thing; it can be used for tests, scans, reports, with the flexibility of calling different Docker images and executing the whole host of commands that might be useful or pertinent.

## Quick Overview for Developers

If you are a developer looking for more information on why there are several "mystery" stages showing up in your pipelines, this section is for you.

In brief, this stages are being called in and executed by an extra `.gitlab-ci.yml` file located in a different place. These stages are all present for quality and/or security purposes, and you shouldn't ever need to alter them. They cannot be skipped or edited.

The Compliance Framework is added to your project by an individual with [Owner](https://docs.gitlab.com/ee/user/permissions.html) rights, and features a blue badge when active.

If any of these stages fail for reasons that are not clear (meaning, something other than an actual code error that was detected in your project), please let the DevOps team know.

## Quick Overview for DevOps/Maintainers

In general, these pull in a selection of Gitlab-provided and custom templates to accomplish the variety of reporting and security tasks it implements. This one template is in use for all different varieties of projects, from TypeScript to Terraform. It uses simple Gitlab CI logic to check for the existence of relevant filetypes.

As of this time, all Compliance Frameworks have been applied manually, using the UI, as this feature is not currently available in Gitlab's IaC platform. There is the possibility of applying by batch using Gitlab's GraphQL API, but it has not been executed yet.

### Update the Compliance Pipeline

In order to make updates to the pipeline (for example, to add an additional scan or remove an existing one), make the edit to the relevant `yml` file, either the `.compliance-ci.yml` file or the template it pulls in. Immediately after committing the change, it will be in force across all the projects that have the Compliance Framework activated.

## Stages

Because the Compliance Pipeline takes precedence over the individual project Gitlab CI, it is required that any stages that are configured in the individual `.gitlab-ci.yml` file are present in the Compliance Pipeline definition. These stages include

- .pre
- test
- build
- deploy
- .post

## Output

Vulnerabilities will be reported on the [Gitlab Vulnerability Report](./gitlab-vulnerability-report.md).
