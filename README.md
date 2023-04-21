# Vagrant Configuration for Kubernetes Installation

Simple vagrant configuration for practicing or experimenting with kubernetes installations

## Requirements

- Vagrant
- VirtualBox

## Usage

Create `config.yaml` and configure number of vms, ip ranges, etc.

```sh
cp config.sample.yaml config.yaml
vi config.yaml
```

Create `bootstrap.sh` and configure provisioning tasks for each vm.

```sh
cp bootstrap.sample.sh bootstrap
vi bootstrap.sh
```

After the above, run the following to start vms.

```sh
vagrant up
```
