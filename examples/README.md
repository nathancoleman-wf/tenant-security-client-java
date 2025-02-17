# SaaS Shield Tenant Security Client Examples

This directory contains some examples of using the Java Tenant Security Client (TSC) SDK to protect sensitive data.

In order to use SaaS Shield, you need to run one or more _Tenant Security Proxies_ (TSPs) in your environment.
This service is provided as a Docker container, so it is easy to run the proxy on any computer that has Docker
installed. IronCore Labs hosts the Docker container on a publicly accessible container registry, so you can pull
the image from there and run it locally.

In addition to the Docker container, you need a configuration file that specifies how the TSP should communicate
with the IronCore Labs Configuration Broker and Data Control Platform, which work together to enable the end-to-end
encryption that keeps all of the tenant KMS configuration information secure. To simplify the process of running
these examples, we have created a demo vendor and tenants that you can use for the examples; all the necessary
configuration information is included in the `demo-tsp.conf` file in this directory.
**NOTE:** Normally, the file containing the configuration would be generated by the vendor and loaded into a
Kubernetes secret or similar mechanism for securely loading the configuration into the docker container. We
have included this configuration in the repository as a convenience. Also note that these accounts are all
created in IronCore's staging infrastructure.

The following commands will get a TSP running on your computer with the provided configuration:

```bash
docker pull gcr.io/ironcore-images/tenant-security-proxy:3.3.0
docker run --env-file demo-tsp.conf -p 32804:7777 -m 512M --mount 'type=bind,src=/tmp,dst=/logdriver' gcr.io/ironcore-images/tenant-security-proxy:3.3.0
```

This starts the TSP locally listening on port 32804.

Once the TSP is running, you can experiment with the example Java programs. Each of the subdirectories contains
a different illustrative example, with instructions to run.

Each of the examples executes as an individual tenant of our demo SaaS vendor. There are six tenants defined;
their IDs are the following:

- tenant-gcp
- tenant-aws
- tenant-azure
- tenant-gcp-l
- tenant-aws-l
- tenant-azure-l

The last three are similar to the first three, but they have [key leasing](https://ironcorelabs.com/docs/saas-shield/what-is-key-leasing/) enabled.

By default, an example will use the `tenant-gcp` tenant. If you would like to experiment with a different tenant, just do:

```bash
export TENANT_ID=<select tenant ID>
```

before running the example.

## Additional Resources

If you would like some more in-depth information, our website features a section of technical
documentation about the [SaaS Shield product](https://ironcorelabs.com/docs/saas-shield/).
