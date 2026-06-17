# Impact and Workarounds for Not Having Private Runners

## Impact

When deploying Azure resources like Storage Accounts and Key Vaults via Terraform without private runners, GitHub-hosted runners operate on public networks and cannot directly access resources deployed within private virtual networks or behind firewall restrictions. This creates several challenges:

- **Dynamic IP addresses**: GitHub-hosted runners use dynamic IPs that cannot be easily allowlisted in Azure firewall rules or Network Security Groups
- **Network isolation issues**: Terraform cannot manage resources that require network-level isolation
- **Key Vault access**: Accessing secrets stored in Key Vaults with IP restrictions becomes problematic

## Workarounds

To mitigate these limitations, consider the following approaches:

### 1. Firewall Exceptions
Temporarily allow Azure services and trusted Microsoft services through firewall exceptions during the Terraform deployment phase. This enables GitHub-hosted runners to access the resources while maintaining some security controls.

### 2. Service Endpoints and Managed Identities
Use service endpoints and managed identities where possible to authenticate without relying on static IP allowlisting. This leverages Azure's identity-based access controls rather than network-based restrictions.

### 3. Two-Phase Deployment
Implement a two-phase deployment approach:
- **Phase 1**: Create network-isolated resources with public access enabled for Terraform management
- **Phase 2**: Lock down resources with stricter network rules in a subsequent step

**Note**: This approach introduces a brief security window that should be minimized and monitored carefully.
