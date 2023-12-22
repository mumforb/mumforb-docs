# Pulumi

The `__main__.py` file is the core of the Pulumi application.

If you are unsure about what Pulumi does, you will (obviously) be best off spending some time with [their documentation](https://www.pulumi.com/docs/). But in (very) brief, Pulumi allows a developer to create and deploy cloud resources using code, and provides libraries for multiple languages. It differs from Terraform mainly in its flexibility, verbosity, and similarity to the rest of the environment a developer works in, allowing greater homogeneity of environments and therefore a smaller learning curve and integration with existing codebases. It also provides tremendous value by exposing tracking, visibility, and statuses via `pulumi.com`, which acts as a sort of Dashboard for all of the organization's infrastructure-as-code deployments.

This doc will assume the usage of the `Python` flavor of Pulumi. Every individual infrastructure project that utilizes Pulumi will operate in a similar fashion.

## Provision the Python `venv` and Install Required Modules

In order to develop using Pulumi, follow these steps:

- Open a new command prompt using VSCode's Terminal menu.
- Run the below commands to prepare the Python virtual environment Pulumi will be using. Make sure to update the `<name-of-repository>`.
  - `$ python3 -m venv /workspaces/<name-of-repository>/venv`
  - `$ /workspaces/<name-of-repository>/venv/bin/python -m pip install -r requirements.txt`
- Your development environment is ready!

## Pulumi Basics

The basic steps and common commands when working with Pulumi:

- `pulumi login` will prompt you for your `pulumi.com` credentials
- `pulumi stack init name-of-stack` will initialize a local instance of a 'stack'
- `pulumi config set aws:region x-xxxx-x` will set a local config file with the appropriate aws region
- `pulumi import xxxxxx` will reach out to an existing resource in AWS and provide a copy/pasteable snippet to enter into your code
- `pulumi preview` will run through the steps included in your Pulumi code and display the list of tasks to be undertaken
- `pulumi up` will execute the code and attempt to deploy real resources in the AWS account associated with the credentials configured
- `pulumi stack output` will display the output configured at `pulumi.export`

When you run `preview` and `up`, what actually happens is that Pulumi creates local virtual environment - in our case in the 'venv' directory - and uses that to actually run the deployment (or mock deployment) tasks. This creates a lot of files in a directory called (in our case) venv. This whole directory should be in the .gitignore, as it's just temporary.

If at some point you see the following message in the bottom-right-hand corner of VSCode, feel free to accept it:
"We've noticed a new workspace folder has been detected. would you like to select it for the container?"

## Reset your Pulumi instance

Sometimes you will deploy real resources to AWS and need to reset and try again. The following steps may help to reset your environment.

- Remove stack
  `pulumi stack rm name-of-stack --force`
- Delete your assets in AWS that were created using the AWS UI
- Re-add stack
  `pulumi stack init name-of-stack`
- Reset region
  `pulumi config set aws:region us-east-2`
- Make your adjustments to the codebase and save
- Do `pulumi preview` to see if everything looks okay
- Do `pulumi up` to provision/update resources

## Helpful Pulumi Commands

Here are a collection of handy commands when you are trying to troubleshoot a problem you're seeing with Pulumi.

### Verbose Logging

Select an number 1 through 9 to determine the level of log level.

- `pulumi up --verbose <int 1-9> --logtostderr`

### Getting the URN of Resources

- `pulumi stack -u`

### Targeting a Specific Resource, Ignoring Others

Just one resource:

- `pulumi up --target 'urn-of-resource-being-targeted'`

Example, with removal of all dependents:

- `pulumi destroy --target "urn:pulumi:administration::core-tooling::eks:index:Cluster::eks-cluster" --target-dependents`

### Refreshing "From" the Remote Stack to Current

- `pulumi refresh --target 'urn:pulumi:administration::core-tooling::gitlab:index/group:Group::name-of-group'`

### Logging Messages to the Console

Pulumi's [log module](https://www.pulumi.com/docs/intro/concepts/logging/) contains several logging methods: info, debug, warn, and error.

```
import pulumi

print_me = "print me!"

pulumi.log.info("print me", print_me)
```
