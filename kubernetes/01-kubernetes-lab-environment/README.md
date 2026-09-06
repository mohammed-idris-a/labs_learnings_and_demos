# Kubernetes Lab Environment ☸️

## Objective

Set up a local Kubernetes learning environment inside an Ubuntu-based virtual machine for hands-on experimentation with containers and Kubernetes.

## Environment

### Virtual Machine Resources

| Resource         |                           Allocation |
| ---------------- | -----------------------------------: |
| CPU              |                              2 vCPUs |
| Memory           |                             8 GB RAM |
| Storage          |                                30 GB |
| Operating System | Ubuntu + Xubuntu desktop environment |

### Software

* Rancher Desktop
* Kubernetes
* `kubectl`
* Homebrew for Linux

### Additional Requirement

Nested virtualization must be enabled because Rancher Desktop is running inside a virtual machine.

## Setup

### 1. Install Xubuntu

The VM was initially configured without a desktop environment.

Xubuntu was installed to provide a lightweight graphical environment for working with Rancher Desktop.

### 2. Install Rancher Desktop

Rancher Desktop was installed to provide a local environment for working with containers and Kubernetes.

#### Issue: Rancher Desktop failed to start

The initial launch failed because nested virtualization was not enabled for the virtual machine.

#### Resolution

Open the VM's processor settings and, under **Virtualization engine**, enable:

**“Virtualize Intel VT-x/EPT or AMD-V/RVI”**

This enables nested virtualization for the VM.

After enabling this option, Rancher Desktop started successfully.

### 3. Install Homebrew

Homebrew was installed for Linux to manage additional tools.

During installation, Homebrew reported that its binary directory was not in the shell's `PATH`.

#### Resolution

The Homebrew environment was added to `.bashrc`:

```bash
echo >> ~/.bashrc
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"' >> ~/.bashrc
```

The environment was then loaded into the current shell:

```bash
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
```

Verify:

```bash
brew --version
```

### 4. Install Kubernetes Tools

Kubernetes tooling was installed after configuring Homebrew.

Verify `kubectl`:

```bash
kubectl version --client
```

Check the current Kubernetes context:

```bash
kubectl config current-context
```

Verify the cluster:

```bash
kubectl get nodes
```

## Troubleshooting

### Homebrew PATH issue

**Symptom**

The Homebrew installer completed, but `brew` was not available from the shell.

**Cause**

The Homebrew environment had not been initialized for the current shell.

**Solution**

```bash
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
```

The configuration was also added to `.bashrc` for future shell sessions.

### Rancher Desktop virtualization issue

**Symptom**

Rancher Desktop would not start inside the VM.

**Cause**

Nested virtualization was disabled.

**Solution**

Nested virtualization was enabled for the VM.

## Key Learnings

* Installed software may not be immediately available if its directory is not in `PATH`.
* Homebrew should be run as the normal user rather than through `sudo`.
* Virtualization-dependent tools may require nested virtualization when running inside a VM.
* Troubleshooting can require investigating multiple layers of the environment rather than only the application itself.
* A reliable Kubernetes lab starts with a correctly configured underlying environment.

## Result

The local Kubernetes environment is operational and ready for hands-on Kubernetes labs.

## Next Lab

**Kubernetes #02 — Pods**

The next lab will create, inspect, modify, and troubleshoot a Kubernetes Pod using `kubectl` and a YAML manifest.

