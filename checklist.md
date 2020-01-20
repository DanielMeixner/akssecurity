# AKS Security Checklist

This is not an exhaustive list and it's work in progress. Use this to find areas which you didn't consider yet.

| Level | Area | Questions to be asked |Guidence  |Solutions |
|-----|-----|------|---|--|
| **Application** *Development* ||
||Software Development Process|
|||Who's allowed to push code?| Restrict permission to only the appropriate group of people | - [Set up permissions in Git]()<br> 
|||What's the process to review code?|Enforce Pull Requests and Code Reviews to double check code.| [PR Flow in Git]() using Azure DevOps<br>[PR Flow in Git]() using GitHub
||External Dependencies|
|||Which external dependencies are allowed? |Consider using a Dependency Scanner to scan for vulnerabilities and license violations for your source code and application | Consider e.g. [Whitesource]()
|||Where are external dependencies pulled from?|        Consider using a private repository | Use [Azure Container Registry]() with RBAC permissions
| **Application** *Deployment* ||
|| Deployment Identity|    Which identity is being used to run a deployment?| Use a non-human service user, automation account or Service principal to run deployments.| [SPN on Azure]()
|| Deployment Workflow |     What is the process of a deployment? |Automate your deployment using a build and release pipeline | Set up Azure [Pipelines]() <br> Set up [GitHub Actions]()
|| Deployment Workflow |    Can you retrace a deployment | Use a pipeline which supports traceability | [GitHub Actions](), <br>Azure [Pipelines]()
|| Deployment Stages |    Do you have multiple copies of the same infrastructure to be able to test your changes on without affecting a user? | Set up multiple environments with similar configuration using Infrastructure as Code | [Terrafrom]()<br>[Azure Resource Manager]()
||  Deployment Infrastructure  |    Who is allowed to make changes in the deployment infrastructure? | Secure your deployment infrastructure | [Azure Resource Locks]() <br> [Azure RBAC]()
| **Application** *Runtime* ||
||Runtime Container Scan | Do the running *containers* contain vulnerabilities or malware?| Install a runtime image scanner | 
||Pulling from Registry|Which registry do you pull images from?| Restrict images to be pulled from specific (private) registries only.<br>Disallow pull from public registries.
|| Secrets| Where do you store secrets?    | Use a secure store which stores secret values (like passwords, certificates) outside of your sorce repos and allows access management. | [Azure Key Vault]()
||Limit SysCalls| What is your running application allowed to do?| Limit potential OS calls of your containerized application by restricting syscalls using. | [AppArmour]()
||Limit Priviliges |  Does your container need advanced privilidges to run? | Forbid advanced priviliges in your container application | [Pod Security Policies]()
| **Networking** *cluster-intern* ||
|| Networking Policies|   Which components are allowed to talk to each other within your cluster?| Restrict internal traffic to only the pods who should be communicating |[Networking Policies]()[Service Mesh]()
|| Encryption / MTLS |   Should traffic within your cluster be encrypted? | Encrypt Pod-to-Pod communication | [Service Mesh]() or [SideCar]()
| **Networking** *cluster-extern* ||
|| Public Endpoints|    Does your cluster need public endpoints? | Disallow public endpoints by integrating your cluster into a private VNET| [Internal LoadBalancer]()
|| Public Endpoints| Is your public endpoint secured using SSL?|    Setup HTTPS endpoints using an ingress controller or gateway for SSL offload | [NGINX]()<br>[Application Gateway]()
|| Public API/Master Endpoint |    Do you need a public endpoint for your Kubernetes API?| Restrict access to the master to traffic from a private vnet | [Private Link]()
|| Ingress Scan|     Which traffic is allowed to access the cluster? | Limit incoming traffic using a Firewall | [Application Gateway WAF]()
||Egress Scan |    Which traffic is allowed to leave the cluster? | Limit egress traffic by locking down your external traffic to specific targets or route via a firewall | [AKS egress]()
||VPN connectivity |    Do you want to access your cluster using a private network?| Integrate your AKS cluster into your on-prem or cloud hosted exisiting network infrastructure | [Kubenet Networking](), [Azure CNI]()
|**Kubernetes Level**
||RBAC, Roles & Rolebindings |What should identites be allowed to do within your cluster|Which Define roles within your cluster with a dedicated set of permissions. | [Kubernetes RBAC]()
||Identity Management|     How do you mangage users of your cluster?| Bind identites from your identity management system to Kubernets Roles | [Azure AD and AKS]()
|**Infrastructure Level**
||OS Security Updates| What's your process for installing new OS versions for your nodes?
||Kubernetes Version Updates|    What's your process for installing new Kubernetes versions| Automate the process of rebooting nodes in case of kernel updates |[Kured]()
||Node Access|     How do you limit remote access to nodes?| Restrict access using Azure RBAC. Access nodes via jumpserver.| [Azure Bastion]()
|**Organization Level**
|| Azure Subscription Management |     Who is allowed to make changes in your Azure subscription / resource group | Set up RBAC in Azure| [Azure RBAC]()
|| Multi cluster management |     How can you enforce your organization to follow security guidelines |Set up Azure Policy to enforce specific rules| [Azure Policy]()
|| Multi cluster management |How can you get notified in case of violations |-  Set up [Azure Security Center ]()for Containers<br> - Set up Azure Arc to ensure consistent configuration of multiple clusters| [Azure Security Center ]()<br>[Azure Arc]()











