# Terratest

Terratest is a unit testing platform for Terraform files. It's a little different than the way we often think about unit tests, due to the fact that Terraform - and declarative IaC code in general - is different than other types of code. In order to test more granular bits of Terraform (e.g. modules) we can only validate the status of the execution by actually deploying real cloud components and checking to make sure they look like we expect. This is how we achieve a full "test" of the code, beyond what the linting and verification packages we use (like Terrascan, KICS, Checkov, and others).

Because Terratest actually deploys real components (however briefly), it is important to keep the AWS account in which we're working separate from the main accounts that contain our actual "stuff". For now, we only have the capability of running Terratest locally (rather than in a CI/CD pipeline) due to our inability to safely distribute AWS login secrets into Gitlab, but this will change soon.

## Run Terratest Locally

If you are using a VSCode `.devcontainer`, you may already have Go installed, which is a prerequisite for running Terratest. In fact, Terratest sits on top of Go's built-in testing platform, so most of the boilerplate setup involves getting Go up and running.

Once Go is installed in your environment, you can execute the following steps, some of which may have already happened, but here's the full list for clarity.

First, log into AWS.
- Note that this step is required, even for non-AWS interacting IaC modules - UNLESS you do not need a secret housed in AWS Secrets Manager OR you supply the secret to your local environment via environment variable. The name for the variable can be found in the Terratest code itself.)

- $ aws sso login

Then, make sure you select the AWS Test Integration account:

- $ aws configure list-profiles # just to see which profiles are available
- $ export AWS_PROFILE=test-integration
- $ aws configure list # verify the test-integration account is selected

Assuming your test files are in a folder named `/test` (named something like `name_of_module_test.go`).

- $ cd test
- $ go mod init gitlab.com/this-is/your-repository # see note
- $ go mod tidy

note
    The `go mod` command initializes a Go module where the command is run. The module name is arbitrary but usually follows a format like github.com/<YOUR_USERNAME>/<YOUR_REPO_NAME>.

warning
    When attempting to follow this process in the EKS repo, the `go.mod` file didn't have anything in the "required" block, which I thought was strange. So I manually copied that over from this repo, and that then triggered the creation of a `go.sum` file, which in turn seemed to activate the dependencies. Not sure what the deal is with that.

Now you should be ready to run the test.

- $ go test -v

You'll see the terminal crank away for a while, as it is actually running `terraform init`, `terraform apply`, and eventually `terraform destroy` under the hood. There will be useful printout to the console to track what's happening. Additionally, if you log into AWS in the browser, you'll be able to see these assets briefly appear and disappear, which may help with troubleshooting your tests.

## Good To Know

- Terratest does best when you test an entire module all at once - all the inputs and outputs, and whatever auxiliary properties can also be verified.
