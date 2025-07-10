# local-policies

This repository contains OpenShift Advanced Cluster Manager (ACM) policies for automating the deployment and configuration of OpenShift clusters.

## Contents

### SR-IOV Network Operator Policy

Located in `policies/`, this policy automatically installs the SR-IOV Network Operator subscription on managed OpenShift clusters.

**Files:**
- `policies/sriov-subscription-policy.yaml` - Main ACM policy for SR-IOV operator installation
- `policies/README.md` - Detailed documentation for the SR-IOV policy
- `policies/kustomization.yaml` - Kustomize configuration for easy deployment

**Features:**
- Automatically creates the `openshift-sriov-network-operator` namespace
- Sets up the required OperatorGroup
- Installs the SR-IOV Network Operator subscription from Red Hat operators
- Targets clusters labeled as SR-IOV capable
- Uses enforce remediation action for automatic installation

**Quick Deploy:**
```bash
# Create the policies namespace
oc create namespace policies

# Deploy using kustomize
oc apply -k policies/

# Or deploy directly
oc apply -f policies/sriov-subscription-policy.yaml
```

## Usage

1. Label your managed clusters to target them for SR-IOV installation:
   ```bash
   oc label managedcluster <cluster-name> sriov-capable=true vendor=OpenShift
   ```

2. Deploy the policy to your ACM Hub cluster

3. Monitor policy compliance and SR-IOV operator installation

For detailed usage instructions, see `policies/README.md`.