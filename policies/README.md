# SR-IOV Subscription Policy

This directory contains an OpenShift Advanced Cluster Manager (ACM) policy that installs the SR-IOV Network Operator subscription on managed OpenShift clusters.

## Overview

The `sriov-subscription-policy.yaml` policy performs the following operations:

1. **Creates the namespace**: `openshift-sriov-network-operator` with appropriate labels and security context
2. **Creates the OperatorGroup**: Required for operator installation in the namespace
3. **Creates the Subscription**: Installs the SR-IOV Network Operator from the Red Hat operator catalog

## Prerequisites

- OpenShift Advanced Cluster Manager (ACM) Hub cluster
- Managed OpenShift clusters with SR-IOV capable hardware
- Managed clusters should be labeled with `sriov-capable: "true"` and `vendor: "OpenShift"`

## Usage

### Deploy the Policy

1. Ensure the `policies` namespace exists on your ACM Hub cluster:
   ```bash
   oc create namespace policies
   ```

2. Apply the policy to your ACM Hub cluster:
   ```bash
   oc apply -f sriov-subscription-policy.yaml
   ```

### Label Target Clusters

Label your managed clusters to target them for SR-IOV installation:

```bash
oc label managedcluster <cluster-name> sriov-capable=true
oc label managedcluster <cluster-name> vendor=OpenShift
```

### Verify Installation

1. Check policy status on the Hub cluster:
   ```bash
   oc get policy -n policies
   ```

2. Check SR-IOV operator installation on managed clusters:
   ```bash
   oc get subscription -n openshift-sriov-network-operator
   oc get csv -n openshift-sriov-network-operator
   ```

## Policy Components

- **Policy**: Main policy definition with remediation action set to `enforce`
- **PlacementBinding**: Binds the policy to the placement rule
- **PlacementRule**: Targets clusters labeled as SR-IOV capable OpenShift clusters

## Customization

You can customize the policy by:

- Modifying the `channel` in the subscription (e.g., `stable`, `4.12`, `4.13`)
- Changing the `installPlanApproval` from `Automatic` to `Manual` for more control
- Adjusting the cluster selector labels in the PlacementRule
- Modifying the `remediationAction` from `enforce` to `inform` for monitoring only

## SR-IOV Configuration

After the operator is installed, you'll need to create additional resources:

1. **SriovNetworkNodePolicy**: Configure SR-IOV devices on nodes
2. **SriovNetwork**: Define SR-IOV networks
3. **NetworkAttachmentDefinition**: Created automatically by SriovNetwork

Refer to the OpenShift SR-IOV documentation for detailed configuration steps. 