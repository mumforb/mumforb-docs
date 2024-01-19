# Pre-Commit

Pre-Commit is "A framework for managing and maintaining multi-language pre-commit hooks."  Pre-commit hooks allow for checking of nearly unlimited number of items before a developer is allowed to successfully make a commit in their local development environment.  These checks can include:

- Running static analyzers, including SAST tools

- Checking for secrets, private SSH keys, and other items that should not be uploaded to source control

- Housekeeping, including cleaning up excess whitespace or inconsistent line endings

- Other linting, including formatting of JSON and YAML files

- Custom testing, including checking for valid JIRA numbers within a commit.  These tests can be written by engineers in Python, Bash, or other scripting languages.

#### Pre-Commit package

Pre-commit is described as "a framework for managing and maintaining multi-language pre-commit hooks."  Information about the pre-commit Python package can be found [here](https://pre-commit.com).

The pre-commit package must be installed:

```python
$ pip install pre-commit
```

Pre-commit can be installed at the root of any project with:

```shell
$ pre-commit install
```


# Terraform technology stack packages

The Terraform stack packages listed below are open source when implemented with pre-commit, and do an excellent job of being complementary in their scans in order to be thorough in results when used in combination with each other.  The tools are [checkov](https://www.checkov.io/), [kics](https://kics.io), [terraform-compliance](https://terraform-compliance.com/), [terrascan](https://runterrascan.io/), [tfsec](https://tfsec.dev), and [tfupdate](https://github.com/minamijoyo/tfupdate).

#### checkov

Checkov by Bridgecrew is a static code analyzer to look for misconfigurations in code before the code is deployed.  This includes Terraform, Helm charts, Kubernetes, and Docker among others.  Checkov can be found [here](https://www.checkov.io).  Checkov can be installed as a pip or brew package.  The brew package seems to run smoother and is recommended for installation:

```bash
$ brew install checkov
```

Checkov policies for Terraform can be found [here](https://www.checkov.io/5.Policy%20Index/terraform.html).  Checkov individual checks or rile checks can be suppressed as found in [this documentation](https://www.checkov.io/2.Basics/Suppressing%20and%20Skipping%20Policies.html).  Please note that comments are required by the DevOps team for rules that have been suppresed.

#### terraform-compliance

Terraform Compliance is "a lightweight, security and compliance focused test framework against Terraform to enable negative testing capability for your infrastructure-as-code."  Terraform Compliance can be installed using pip with the command:

```bash
$ pip install terraform-compliance
```

More information about Terraform Compliance can be found [here](https://terraform-compliance.com/).

#### terrascan

Terrascan is another static analysis security tool which helps to "detect compliance and security violations across Infrastructure as Code (IaC) to mitigate risk before provisioning cloud native infrastructure" and can be found [here](https://runterrascan.io/).

Terrascan can be installed with the following command:

```bash
$ brew install terrascan
```

Terrascan policies for AWS can be found [here](https://runterrascan.io/docs/policies/aws/).  Rule skipping documentation is available in [this documentation](https://runterrascan.io/docs/usage/in-file_instrumentation/).

#### tfsec
Tfsec is another static analysis scanner for Terraform code supported by Aqua Security and can be found [here](https://tfsec.dev).  It uses OPA (Rego) to define policies as well.

Tfsec can be installed with the following command:

```bash
$ brew install tfsec
```

TFSec policies can be found [here](https://aquasecurity.github.io/tfsec/v1.27.1/checks/aws/).  Ignoring checks can be found with [this documentation](https://aquasecurity.github.io/tfsec/v1.27.1/guides/configuration/ignores/).

#### tfupdate

TFUpdate is a utility to update version constraints of Terraform core, providers, and modules.  The utility can be found [here] (https://github.com/minamijoyo/tfupdate) and can be installed with the following command:

```bash
$ brew install tfupdate
```

#### Pre-Commit Configuration File

Below is an example of the _.pre-commit-config.yaml_ file to be placed within a Terraform external module.  Additional tests for infrastructure repos that utilize external modules would be [Infracost](https://infracost.io).

```yaml
repos:
- repo: https://github.com/gruntwork-io/pre-commit
  rev: v0.1.17 # Get the latest from: https://github.com/gruntwork-io/pre-commit/releases
  hooks:
    - id: terraform-fmt
    - id: terraform-validate
    - id: tflint
      args:
        - "--config=.tflint.hcl"
        - "--enable-rule=terraform_documented_variables"
        - "--module"
- repo: https://github.com/antonbabenko/pre-commit-terraform
  rev: v1.74.1 # Get the latest from: https://github.com/antonbabenko/pre-commit-terraform/releases
  hooks:
    - id: tfupdate
      name: Autoupdate Terraform versions
    - id: tfupdate
      name: Autoupdate AWS provider versions
      args:
      - --args=provider aws # Will be pined to latest version
    - id: terraform_checkov
    - id: terraform_docs
    - id: terraform_tfsec
    - id: terrascan
      args: [""]
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v4.3.0
  hooks:
    - id: check-added-large-files
    - id: check-ast
    - id: check-case-conflict
    - id: check-executables-have-shebangs
    - id: check-json
    - id: check-merge-conflict
    - id: check-shebang-scripts-are-executable
    - id: check-symlinks
    - id: check-toml
    - id: check-xml
    - id: check-yaml
    - id: destroyed-symlinks
    - id: detect-aws-credentials
      args: ["--allow-missing-credentials"]
    - id: detect-private-key
    - id: end-of-file-fixer
    - id: fix-byte-order-marker
    - id: mixed-line-ending
      args: [--fix=lf]
    - id: pretty-format-json
    - id: sort-simple-yaml
    - id: trailing-whitespace
```

To update above referenced versions to be sure the latest is used, please use the command:

```shell
$ pre-commit autoupdate
```
Pre-commit can be test run with the command:

```shell
$ pre-commit run -a
```
