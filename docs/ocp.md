# OpenShift Wrapper

The kube-burner binary brings a very opinionated OpenShift wrapper designed to simplify the execution of different workloads in this kubernetes distribution.
This wrapper is hosted under the `kube-burner ocp` subcommand that currently looks like:

```console
$ kube-burner ocp
This subcommand is meant to be used against OpenShift clusters and serve as a shortcut to trigger well-known workloads

Usage:
  kube-burner ocp [command]

Available Commands:
  cluster-density    Runs cluster-density workload
  node-density       Runs node-density workload
  node-density-heavy Runs node-density-heavy workload
  node-density-cni   Runs node-density-cni workload

Flags:
      --burst int          Burst (default 20)
      --es-index string    Elastic Search index
      --es-server string   Elastic Search endpoint
  -h, --help               help for ocp
      --qps int            QPS (default 20)
      --uuid string        Benchmark UUID (default "ff60bd1c-df27-4713-be3e-6b92acdd4d72")

Global Flags:
      --log-level string   Allowed values: trace, debug, info, warn, error, fatal (default "info")

Use "kube-burner ocp [command] --help" for more information about a command.

```

## Usage

In order to trigger one of the supported workloads using this subcommand you have to run kube-burner using the subcommand ocp. The workloads are embed in the kube-burner binary:

Running node-density with 100 pods per node

```console
$ kube-burner ocp node-density --pods-per-node=100
$
```

With the command above, the wrapper will calculate the required number of pods to deploy across all worker nodes of the cluster.

This wrapper provides the following benefits among others:

- Provides a simplified execution of the supported workloads
- Indexes OpenShift metadata along with the Benchmark result, this document can be found with the following query: `uuid: <benchmkark-uuid> AND metricName.keyword: "clusterMetadata"`
- Prevents modifying configuration files to tweak some of the parameters of the workloads
- Discovers the Prometheus URL and authentication token, so the user does not have to perform those operations before using them.

## Customizing workloads

It's possible to customize the workload configuration before running the workload by extracting, updating and finally running it:

```console
$ kube-burner ocp node-density --extract
$ ls
alerts.yml  metrics.yml  node-density.yml  pod.yml
$ vi node-density.yml                               # Perform modifications accordingly
$ kube-burner ocp node-density --pods-per-node=100  # Run workload
```
