# CDW Managed Services Kyverno Policies
These Kyverno policies are targeted for deployment to CDW Managed Services
Kubernetes clusters.  The intent is to use GitOps with
[kustomize](https://kustomize.io/) to install.

## How things are organized
Kustomize provides a solution for customizing Kubernetes resource configuration
free from templates and DSLs. It lets you customize raw, template-free YAML files
for multiple purposes, leaving the original YAML untouched and usable as is.

With this in mind, the simplest approach to understanding "how things work" is
to treat `kustomization.yaml` files as bread crumbs. That is to say, where you
find a `kustomization.yaml` file, it will take you to your next
`kustomization.yaml`...which in turn will take you to another
`kustomization.yaml`...etc., etc.

A common practice is to use `overlays` directories to model your different
environments (i.e. kubernetes clusters).  Following this pattern, our various
kubernetes clusters will have policy defined under overlays directories.

### Merge from Upstream
Any time we merge changes from the upstream, public repository we should
expect breaking changes. Currently, there's two set of upstream policies
that are heavily used by CDW:

* best-practices
* pod-security

You will find these directories in the root of the repository. We've added
a `kustomization.yaml` in each of these directories. The `kustomization.yaml`
file in these directories simply includes all the policies found in the
directory.

This enables kustomize to work correctly across the logical boundary between 
"upstream" and CDW's own copy of these policies. The term "copy" in this
context means we've created a symlink of these directories under the `cdw/mans`
subdirectories. For example, if you look under `cdw/mans/best-practices/`, you
will notice that the "baseline" subdirectory is a symlink to the upstream
"best-practices" directory. And in turn, the `kustomization.yaml` in this
directory references "baseline" to make kustomize magically work.

So another important point to call out is that from both the perspective of
kustomize, as well as operationally, "baseline" should always reflect what's in
upstream. And then we create kustomize overlays to align our policies with the
varying requirements of our clusters.

Because of the "bread crumbs" nature of this setup, it's important to undestand
these places where Kustomize could break. While it's perhaps a bit brittle, the
current setup seems to work sufficiently.

The final point to emphasize is that after you merge from upstream, you should
perform some quick-and-dirty tests to make sure kustomize still renders the
policies.

## Quick-and-dirty test
To see kustomize in action, just find a directory with a `kustomization.yaml`
file. To render Kubernetes manifests with kustomize, you execute a build
command against that directory, like so:

```sh
bash-5.0$ kustomize build cdw/mans/overlays/eval
```

