# Stripe Terraform Provider

The Stripe Terraform provider uses the official Stripe SDK based on Golang. On top of that, the provider is developed
around the official Stripe API documentation [website](https://stripe.com/docs/api).

The Stripe Terraform Provider documentation can be found on the Terraform Provider documentation [website](https://registry.terraform.io/providers/umisora/stripe/latest).

## Usage:

```terraform
terraform {
  required_providers {
    stripe = {
      source = "umisora/stripe"
    }
  }
}

provider "stripe" {
  api_key="<api_secret_key>"
}
```

### Environmental variable support

The parameter `api_key` can be omitted when the `STRIPE_API_KEY` environmental variable is present.

### Local Debug

- With Terraform Cloud

If you want to test new code with code already running on Terraform Cloud:

- Build the provider with `go build main.go`
- Move the final binary to the `mv main ~/.terraform.d/plugins/local/umisora/stripe/100/darwin_amd64/terraform-provider-stripe_v100`
- Actually run the code using the Provider code locally. Change the backend from remote to local.

```
terraform {
  backend "local" {
    path = "terraform.tfstate"
  }
  # backend "remote" {
  #   hostname     = "app.terraform.io"
  #   organization = "xxx"

  #   workspaces {
  #     prefix = "xxxx-"
  #   }
  # }
}
```

- Run `terraform init -migrate-state.` Answer "Yes" when prompted to copy (migrate) tfstate from remote to local.
- Point the provider to local.

```
terraform {
  # Providers
  required_providers {
    stripe = {
      //source = "umisora/stripe"
      source  = "terraform.local/umisora/stripe"
      version = "~> 1.3.1"
    }
  }
  # Terrafirn Versuib
  required_version = "~> 1.0.0"
}
```

- Run `terraform plan`

If you are using TFENV and encounter an error during Init due to Apple Silicon, reinstall Terraform.
tfenv uninstall 1.0.10
TFENV_ARCH=amd64 tfenv install 1.0.10
