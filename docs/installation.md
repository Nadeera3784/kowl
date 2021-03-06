# Installation & Deployment

This page describes how to install Kowl (Business) on your platform.

## Docker images

Docker images are available on Quay.io for [Kowl](https://quay.io/repository/cloudhut/kowl?tab=tags) and [Kowl Business](https://quay.io/repository/cloudhut/kowl-business?tab=tags). We comply with semantic versioning and every new release is pushed to Quay. Additionally we publish a new docker image after each push to master.

## Binaries

We do not provide any precompiled binaries as of now. For Kowl you can obviously build them on your own by mimicing the steps in the [Dockerfile](../Dockerfile). If you want us to provide binaries please let us know in the issues. If there's enough interest we'll provide them in the future as part of our release process.

## OAuth Setup

Kowl Business supports Authentication and Authorization. In order to configure OAuth you first must setup the OAuth application in your desired identity provider:

- [Google OAuth setup](./provider-setup/google.md#google-oauth-provider-setup)
- GitHub OAuth setup (TODO)

## Deployment

While Kowl was written to be deployed in Kubernetes it does not have any specific dependencies. You can run it with any other container orchestrator or on bare metal.

### Kubernetes

We maintain a [Helm chart](https://github.com/cloudhut/charts) as well as a [Terraform module](https://github.com/cloudhut/terraform-modules) which makes it easy to deploy Kowl or it's business version on Kubernetes. Please refer to these repos for further documentation and/or help.

If you want to create your own manifest you can either browse through the Helm chart or Terraform module linked above or refer to the [below section](#other).

### Other

Kowl must be configured using flags and a YAML config file. Flags must be used to set the path to your YAML config file. Additionally you can use flags for all sensitive input (such as Kafka passwords). All available flags along with a reference YAML config are described below.

If your Kafka cluster requires TLS authentication you can make certificates available via volume mounts. Afterwards you need to configure the paths to your certificates in the YAML config as shown in the reference config (see section [config files](#config-files)).

#### Flags

In general we only use flags to specify the path to your config file which contains all the actual configuration. Because Kowl requires sensitive information such as credentials, we offer further flags so that you don't need to put them into your YAML configuration. This way the YAML config can be pushed into your repository while sensitive information can be mounted during the deployment process.

##### Kowl

As the Business version is an extension of the normal Kowl version these flags apply to Kowl **and** Kowl Business.

| Argument | Description | Default |
| --- | --- | --- |
| --config.filepath | Path to the config file | (No default) |
| --kafka.sasl.password | SASL Password | (No default) |
| --kafka.sasl.gssapi.password | Kerberos password if auth type user auth is used | (No default) |
| --kafka.tls.passphrase | Passphrase to decrypt the TLS key (leave empty for unencrypted key files) | (No default) |

##### Kowl Business

| Argument | Description | Default |
| --- | --- | --- |
| --cloudhut.license-token | CloudHut license token which is issued after registration at cloudhut.dev | (No default)
| --login.google.client-secret | OAuth Client Secret for Google | (No default, optional)
| --login.github.client-secret | OAuth Client Secret for GitHub | (No default, optional)

#### Config Files

All available settings which can be configured in your YAML configs can be found here and should document themselves:

[/docs/config/kowl.yaml](./config/kowl.yaml)

[/docs/config/kowl-business.yaml](./config/kowl-business.yaml)

[/docs/config/kowl-business-roles.yaml](./config/kowl-business-roles.yaml)

[/docs/config/kowl-business-role-bindings.yaml](./config/kowl-business-role-bindings.yaml)