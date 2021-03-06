Terraform Provider

This is a plugin for HashiCorp [Terraform](https://terraform.io/), which helps creates, configures and deletes topics on  on [Kafka](http://kafka.apache.org/).

==================

- Website: https://www.terraform.io
- [![Gitter chat](https://badges.gitter.im/hashicorp-terraform/Lobby.png)](https://gitter.im/hashicorp-terraform/Lobby)
- Mailing list: [Google Groups](http://groups.google.com/group/terraform-tool)


Maintainers
-----------

This provider plugin is maintained by Xi Ning Wang (https://github.com/osswangxining/terraform-provider-ibm).

Requirements
------------

-	[Terraform](https://www.terraform.io/downloads.html) 0.10.x
-	[Go](https://golang.org/doc/install) 1.8 (to build the provider plugin)

Building The Provider
---------------------

Clone repository to: `$GOPATH/src/github.com/terraform-providers/terraform-provider-$PROVIDER_NAME`

```sh
$ mkdir -p $GOPATH/src/github.com/terraform-providers; cd $GOPATH/src/github.com/terraform-providers
$ git clone git@github.com:terraform-providers/terraform-provider-$PROVIDER_NAME
```

Enter the provider directory and build the provider

```sh
$ cd $GOPATH/src/github.com/terraform-providers/terraform-provider-$PROVIDER_NAME
$ make build
```

Using the provider
----------------------
see <https://www.terraform.io/docs/providers/http/data_source.html>


- Download the plugin.
- [Install](https://terraform.io/docs/plugins/basics.html) it, or put into a directory with configuration files.
- Create a minimal terraform template file.  There is an example in `sample/`.
- Modify the settings in the provider and the topic settings in the `kafka_topic` resource.
- Run:
```
$ terraform apply
```

- Example configuration:

```hcl
provider "kafka" {
  bootstrap_servers = ["localhost:9092"]
}

resource "kafka_topic" "logs" {
  name               = "systemd_logs"
  replication_factor = 2
  partitions         = 100

  config = {
    "segment.ms" = "20000"
  }
}
```


Developing the Provider
---------------------------

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.8+ is *required*). You'll also need to correctly setup a [GOPATH](http://golang.org/doc/code.html#GOPATH), as well as adding `$GOPATH/bin` to your `$PATH`.

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make bin
...
$ $GOPATH/bin/terraform-provider-$PROVIDER_NAME
...
```

In order to test the provider, you can simply run `make test`.

```sh
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run.

```sh
$ make testacc
```

See the development guide - Writing Custom Providers
---------------------------
https://www.terraform.io/guides/writing-custom-terraform-providers.html


How to debug terraform #16752
---------------------------
```sh
set TF_LOG=DEBUG
set TF_TF_LOG_PATH=/tmp/log
terraform apply
observe TRACE level logs in the file /tmp/log
```