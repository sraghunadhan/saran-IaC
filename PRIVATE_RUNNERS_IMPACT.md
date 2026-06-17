# Impact and Workarounds for Not Having Private Runners

## Impact

When deploying Azure resources like Storage Accounts and Key Vaults via Terraform without private runners, GitHub-hosted runners operate on public networks and cannot directly access resources deployed within private virtual networks or behind firewall restrictions. This creates security and connectivity challenges, as the runners' dynamic IP addresses cannot be easily allowlisted in Azure firewall rules or Network Security Groups, potentially preventing Terraform from managing resources that require network-level isolation or from accessing secrets stored in Key Vaults with IP restrictions enabled.

## Workarounds

To mitigate these limitations, you can temporarily allow Azure services and trusted Microsoft services through firewall exceptions during the Terraform deployment phase, or use service endpoints and managed identities where possible to authenticate without relying on static IP allowlisting. Additionally, consider implementing a two-phase deployment approach where network-isolated resources are initially created with public access enabled for Terraform management, then locked down with stricter network rules in a subsequent step, though this introduces a brief security window that should be minimized and monitored.
