Terraform Provider for PostgreSQL
=================================

This provider allows you to manage [PostgreSQL](https://www.postgresql.org/) objects like databases, extensions, roles, and more using Terraform.

It's published on the [Terraform Registry](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs).
It replaces the [original HashiCorp provider](https://github.com/hashicorp/terraform-provider-postgresql) since HashiCorp stopped hosting community providers in favor of the Terraform Registry.

- **Documentation**: https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs
- **Changelog**: [CHANGELOG.md](./CHANGELOG.md)

## Requirements

-	[Terraform](https://www.terraform.io/downloads.html) 0.12.x or later
-	[Go](https://golang.org/doc/install) 1.16 or later (to build the provider plugin)

## Building The Provider

Clone the repository to: `$GOPATH/src/github.com/cyrilgdn/terraform-provider-postgresql`

```bash
$ mkdir -p $GOPATH/src/github.com/cyrilgdn; cd $GOPATH/src/github.com/cyrilgdn
$ git clone git@github.com:cyrilgdn/terraform-provider-postgresql
```

Enter the provider directory and build the provider using the [`Makefile`](./Makefile):

```bash
$ cd $GOPATH/src/github.com/cyrilgdn/terraform-provider-postgresql
$ make build
```

## Using the Provider

Usage examples can be found in the [provider documentation](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs) and in the [`examples`](./examples) directory.

## Developing the Provider

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.11+ is *required*). You'll also need to correctly set up a [GOPATH](http://golang.org/doc/code.html#GOPATH), as well as adding `$GOPATH/bin` to your `$PATH`.

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```bash
$ make build
...
$ $GOPATH/bin/terraform-provider-postgresql
...
```

### Testing

To test the provider, you can simply run `make test`:

```bash
$ make test
```

To run the full suite of acceptance tests, run `make testacc`:

**Note:** Acceptance tests create real resources, and often cost money to run.

```bash
$ make testacc
```

### Manual Testing

To manually run some acceptance tests locally, use the following commands:

```bash
# Spins up a local Docker PostgreSQL container
make testacc_setup 

# Load the needed environment variables for the tests
source tests/switch_superuser.sh

# Run the test(s) that you're working on as often as you want
TF_LOG=INFO go test -v ./postgresql -run ^TestAccPostgresqlRole_Basic$

# Cleans the environment and tears down the PostgreSQL container
make testacc_cleanup 
```

The test setup and cleanup scripts can be found in the [`tests`](./tests) directory:
- [`testacc_setup.sh`](./tests/testacc_setup.sh) - Sets up the test environment
- [`testacc_cleanup.sh`](./tests/testacc_cleanup.sh) - Cleans up the test environment
- [`switch_superuser.sh`](./tests/switch_superuser.sh) - Switches to superuser mode for testing
