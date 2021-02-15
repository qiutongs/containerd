# Getting Started for Developers

This document provides steps to build `cri` plugin and test it locally.

## Install Dependencies

1. Follow this [guide](https://github.com/containerd/containerd/blob/master/BUILDING.md) and install required dependencies.
2. Install [CNI](https://github.com/containernetworking/cni) by following its homepage.

## Build and Install cri

When building the containerd, cri is automatically being built unless you specify no_cri build tag. For more detail, see this [guide](https://github.com/containerd/containerd/blob/master/BUILDING.md#build-containerd)

### Build Tags

`cri` supports optional build tags for compiling support of various features.
To add build tags to the make option the `BUILD_TAGS` variable must be set.

```bash
make BUILDTAGS='seccomp apparmor selinux'
```

| Build Tag | Feature                            | Dependency                      |
|-----------|------------------------------------|---------------------------------|
| seccomp   | syscall filtering                  | libseccomp development library  |
| selinux   | selinux process and mount labeling | <none>                          |
| apparmor  | apparmor profile support           | <none>                          |

### Running a Kubernetes local cluster

If you already have a working development environment for supported Kubernetes
version, you can try `cri` in a local cluster:

0. If containerd is already running as a daemon service, you need to stop it first.
```bash
sudo service containerd stop
```

1. Start the version of `containerd` with `cri` plugin that you built and installed
above as root in a first terminal:
```bash
sudo containerd
```
2. From the Kubernetes project directory startup a local cluster using `containerd`:
```bash
sudo CONTAINER_RUNTIME=remote CONTAINER_RUNTIME_ENDPOINT='unix:///run/containerd/containerd.sock' PATH=$PATH ./hack/local-up-cluster.sh
```

For more detail, refer to [the guide of running locally in Kubernetes](https://github.com/kubernetes/community/blob/master/contributors/devel/running-locally.md).

### Test

See [here](./testing.md) for information about test.