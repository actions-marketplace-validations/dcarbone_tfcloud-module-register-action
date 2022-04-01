# Terraform Cloud Module Registration Action
GitHub action you can use to register a module within Terraform Cloud

# Inputs

## Required
* `token` - Terraform Cloud API Token with at least "Manage Modules" permissions.
* `organization` - Name of Terraform Cloud organization
* `namespace` - Your organization's namespace
* `module-name` - Name of module
* `provider-name` Name of primary provider used by module

## Optional

* `registry-name` - Name of Registry to push to
  * Defaults to `private`

# Outputs

* `module` - JSON response from one of:
  * https://www.terraform.io/cloud-docs/api-docs/private-registry/modules#create-a-module-with-no-vcs-connection
    * Returned when module is registered with this action
  * https://www.terraform.io/cloud-docs/api-docs/private-registry/modules#get-a-module
    * Returned with module was already registered
* `errors` - Any error seen during execution

# Example

```yaml
name: "Register Module"

# if configured as such, this action will only trigger when a new branch is 
# created with a `v` prefix
on:
  push:
    tags:
      - 'v*'

jobs:
  register-module:
    runs-on: ubuntu-latest
    steps:
      - uses: dcarbone/tfcloud-module-register-action@v0.1.0
        with:
          token: ${{ secrets.TFCLOUD_API_KEY }}
          organization: { your organization }
          namespace: { your organization namespace }
          module-name: { name of module }
          provider-name: { name of primary provider used by module }
```