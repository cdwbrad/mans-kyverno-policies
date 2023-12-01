# CDW Managed Services Kyverno Policies
These Kyverno policies are targeted for deployment to CDW Managed Services
Kubernetes clusters.

## Approach
Given the relative inexperience of our Kubernetes team, especially around
"policy-as-code", it makes sense to learn as we go.

With this in mind, the approach with Kyverno is to use the upstream policies
as a starting point (e.g. best-practices). And then augment with our own
custom policies as required.

This rationale will help you better understand how the repository is organized
to both track upstream changes as well as manage CDW Managed Services' own
policies.

## Learn more, the details...
Go these [docs](./docs/README.md) for a more in-depth review.

