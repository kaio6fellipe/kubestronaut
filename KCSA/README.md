# The journey to KCSA certification

## Day 1 (20 Dec 2025)

KCSA course started at Kodekloud. Security is a special topic for me, I always had difficulties with details that I always forget about operacional system security components around the kernel and security around containered environments.

- Overview of Cloud Native Security Quiz: 89%

- 14% completed, 86% to go.

## Day 2 (22 Dec 2025)

Securing Kubelet, container runtime, kube-proxy, etc, container networking and client security around kubectl proxy and kubeconfig.

- 30% completed, 70% to go.

## Day 3 (23 Dec 2025)

Kubernetes Security Fundamentals, Threat Model, Compliance and Security Frameworks.

- 72% completed, 28% to go.

## Day 4 (24 Dec 2025)

Platform Security, minimizing base image blueprint, image scanning, repository security, observability, falco overview, using falco to detect threats and review of service mesh with Istio.

- 88% completed, 12% to go.

## Day 5 (26 Dec 2025)

Finishing the Platform Security topic, review of kubernetes PKI and TLS.

- 100% completed.

- First mock exam: 72% 😥
  - [ ] Review of STRIDE
  - [ ] Review of the responsibilities around Pod Security Policies and Admission Controllers

## Day 6 (27 Dec 2025)

Just found this mock exam app for KCSA: https://kubernetes-security-kcsa-mock.vercel.app/, special thanks to [@thiago4go](https://github.com/thiago4go) for creating it.

- Second mock exam (160 questions): 89%

Score by Domain:

- Cloud Native Security: 14 / 18 (77.8%)
- Compliance and Security Frameworks: 16 / 16 (100.0%)
- Kubernetes Cluster Component Security: 42 / 47 (89.4%)
- Kubernetes Security Fundamentals: 46 / 52 (88.5%)
- Kubernetes Threat Model: 10 / 11 (90.9%)
- Platform Security: 14 / 16 (87.5%)

### Questions to review

1. When configuring encryption at rest for Kubernetes secrets using etcd, which Kubernetes component's configuration must be updated to enable this encryption?

    > Your answer: `etcd`
    >
    > Correct answer: `kube-apiserver`

    Explanation: Encryption at rest for Kubernetes secrets is configured by updating the kube-apiserver with an encryption configuration file. The kube-apiserver handles requests and encrypts secrets before storing them in etcd. The other components do not manage encryption configuration for secrets.

    > Domain: Kubernetes Cluster Component Security

2. Which Kubernetes resources, if compromised, could lead to significant security breaches and should be audited carefully? (Select all that apply)

    > Your answer: `Secrets, ServiceAccounts, Pods`
    >
    > Correct answer: `Secrets, ServiceAccounts`

    Explanation: Secrets and ServiceAccounts are considered sensitive Kubernetes resources because they often contain confidential information such as credentials, API keys, and tokens. If compromised, these resources can lead to unauthorized access, privilege escalation, or data breaches. ConfigMaps generally contain non-sensitive configuration data and are less critical from a security perspective. While Pods and PersistentVolumes are important, they are not typically audited for sensitive information unless they handle sensitive data explicitly.

    > Domain: Kubernetes Security Fundamentals

3. What is the primary difference between MicroVM and User-Space Kernel approaches in cloud native security?

    > Your answer: `MicroVMs are larger in size`
    >
    > Correct answer: `User-space kernels intercept system calls in user space`

    Explanation: The primary difference lies in how they operate. User-space kernels, like gVisor, run in user space and intercept system calls, providing a lightweight sandboxing mechanism. MicroVMs, on the other hand, are full-fledged virtual machines that run directly on hardware but require a host OS for management.

    > Domain: Cloud Native Security

4. Which Kubernetes object is used to persist data across the lifecycle of a pod?

    > Your answer: `PersistentVolumeClaim`
    >
    > Correct answer: `PersistentVolume`

    Explanation: A PersistentVolume (PV) is a Kubernetes resource that provides persistent storage for data that needs to be preserved across pod lifecycles. Unlike data stored in an EmptyDir or other ephemeral storage, data in a PersistentVolume remains available even if the pod is deleted or recreated. PersistentVolumeClaims (PVCs) are used to request storage resources from PersistentVolumes, but the PV itself is the resource that holds the data.

    > Domain: Kubernetes Cluster Component Security

5. Which command calculates the SHA256 checksum of the file '/usr/bin/kubelet' on a Linux system?

    > Your answer: `checksum -sha256 /usr/bin/kubelet`
    > 
    > Correct answer: `sha256sum /usr/bin/kubelet`

    Explanation: The command 'sha256sum /usr/bin/kubelet' computes the SHA256 checksum of the specified file, which is commonly used to verify file integrity. 'sha1sum' and 'md5sum' calculate different hash algorithms (SHA1 and MD5 respectively). 'checksum -sha256' and 'hash -a sha256' are not standard Linux commands for checksum calculation.

    > Domain: Platform Security

6. Which Kubernetes resource type allows cluster administrators to define and enforce the use of specific container runtimes like gVisor or Kata Containers?

    > Your answer: `ContainerRuntime`
    >
    > Correct answer: `RuntimeClass`

    Explanation: RuntimeClass is a first-class Kubernetes resource that specifies container runtime configurations. It enables security-conscious runtime selection (like gVisor's sandboxed environment) while maintaining portability across different container runtimes. PodSecurityPolicy (deprecated in v1.21) handled security policies but not runtime selection.

    > Domain: Kubernetes Cluster Component Security

7. In a Kubernetes container's securityContext, which field allows you to add specific Linux capabilities to the container?

    > Your answer: `addCapabilities`
    >
    > Correct answer: `capabilities.add`

    Explanation: The correct field is 'capabilities.add', which is a list under the container's securityContext that specifies which Linux capabilities to add to the container's default set. 'addCapabilities' and 'linuxCapabilities' are incorrect field names. 'securityOptions' and 'privilegedCaps' are not valid Kubernetes securityContext fields.

    > Domain: Kubernetes Security Fundamentals

8. In Kubernetes Pod specifications, which field explicitly defines the container runtime class that should be used for pod execution?

    > Your answer: `containerRuntimeClass`
    >
    > Correct answer: `runtimeClassName`

    Explanation:The 'runtimeClassName' field in Pod specifications references a RuntimeClass object that defines container runtime configurations. This allows administrators to control runtime security parameters and isolation mechanisms. Other options like 'runtimeClass' or 'containerRuntimeClass' are invalid field names in the Kubernetes API.

    > Domain: Kubernetes Cluster Component Security

9. Which Docker flag allows a container to share the PID namespace with another container named 'container1'?

    > Your answer: `--namespace=pid:container1`
    >
    > Correct answer: `--pid=container:container1`

    Explanation: The correct flag to share the PID namespace is '--pid=container:container1'. This allows processes in one container to see and interact with processes in the other container.

    > Domain: Cloud Native Security

10. Which commands can be used to list all loaded AppArmor profiles on a Linux node?

    > Your answer: `sudo aa-enforce, sudo apparmor_parser -L`
    >
    > Correct answer: `sudo apparmor_status, sudo aa-status`

    Explanation: Both `sudo apparmor_status` and `sudo aa-status` can be used to list all loaded AppArmor profiles on a Linux node. These commands provide detailed information about AppArmor, including the number of loaded profiles, their modes (enforce or complain), and any unconfined processes. Other options are incorrect for the following reasons: 1. `sudo aa-enforce`: This command is used to set an AppArmor profile to enforce mode, not to list profiles. 2. `sudo apparmor_parser -L`: This command is used for loading AppArmor profiles but does not list them. 3. `sudo systemctl status apparmor`: This command checks the status of the AppArmor service but does not provide a list of loaded profiles. Using either `sudo apparmor_status` or `sudo aa-status` ensures you can monitor and manage AppArmor profiles effectively.

    > Domain: Platform Security

11. Which STRIDE threat category primarily addresses threats related to data integrity?

    > Your answer: `Information Disclosure`
    >
    > Correct answer: `Tampering`

    Explanation:The 'Tampering' category in STRIDE focuses on threats that compromise data integrity by unauthorized modification of data. Spoofing relates to identity, repudiation to denial of actions, information disclosure to confidentiality breaches, and denial of service to availability issues.

    > Domain: Kubernetes Threat Model

12. Which authentication mechanisms are commonly used by kubeadm during Kubernetes cluster bootstrapping? (Select all that apply)

    > Your answer: `TLS certificates, Token-based authentication, OAuth tokens`
    >
    > Correct answer: `TLS certificates, Token-based authentication`

    Explanation: kubeadm primarily uses TLS certificates and token-based authentication mechanisms to secure communication and authenticate components during cluster bootstrapping. TLS certificates are used for API server and client authentication, while bootstrap tokens facilitate node joining.

    > Domain: Kubernetes Security Fundamentals

13. How can you retrieve the digest (content hash) of the Docker image 'nginx:1.19' using the Docker CLI?

    > Your answer: `docker inspect nginx:1.19 --format='{{.Id}}'`
    >
    > Correct answer: `docker inspect --format='{{index .RepoDigests 0}}' nginx:1.19`

    Explanation: The command 'docker inspect --format='{{index .RepoDigests 0}}' nginx:1.19' extracts the image digest from the image's metadata, showing the content-addressable identifier (digest) of the image. Option 1 lists digests for all images but may not show the specific digest for the tag. Option 2 shows the image ID, which is different from the digest. Option 3 is incomplete and incorrect syntax. Option 5 is unrelated to obtaining the digest, as it tags images but does not display digests.

    > Domain: Cloud Native Security

14. Which Kubernetes resource is commonly used to integrate Open Policy Agent (OPA) for policy enforcement through admission control?

    > Your answer: `AdmissionController`
    >
    > Correct answer: `ValidatingWebhookConfiguration`

    Explanation: The 'ValidatingWebhookConfiguration' resource is used to register admission webhooks that validate requests to the Kubernetes API server. Open Policy Agent (OPA) leverages this mechanism to enforce custom policies by intercepting and validating API requests. 'MutatingWebhookConfiguration' is used for modifying requests, 'AdmissionController' is a general concept, 'CustomResourceDefinition' defines new resource types, and 'PolicyController' is not a standard Kubernetes resource.

    > Domain: Kubernetes Security Fundamentals

15. How can you prevent pods from being scheduled on a Kubernetes node?

    > Your answer: `kubectl cordon <node-name>`
    >
    > Correct answer: `kubectl cordon <node-name>, kubectl taint nodes <node-name> key=value:NoSchedule`

    Explanation:Both cordoning and tainting a node with `kubectl taint nodes <node-name> key=value:NoSchedule` prevents pods without a matching toleration from being scheduled on it. This is a way to mark nodes as unsuitable for certain workloads without completely removing them from the cluster.

    > Domain: Kubernetes Cluster Component Security

16. What are the key characteristics of gVisor and Firecracker? (Select all that apply)

    > Your answer: `Firecracker uses lightweight microVMs with KVM, Both provide full hardware virtualization`
    >
    > Correct answer: `gVisor is a user-space kernel providing sandboxing, Firecracker uses lightweight microVMs with KVM, gVisor offers better performance than traditional VMs`

    Explanation:gVisor provides sandboxing by running in user space and intercepting system calls, which enhances security without the need for full hardware virtualization. Firecracker uses microVMs, leveraging KVM for efficient isolation. gVisor can offer better performance compared to traditional VMs due to its lightweight nature.

    > Domain: Cloud Native Security

## Day 7 (29 Dec 2025)

Review of previous questions and focused on mock exams around my weaknesses topics.

- Mock exam for Cloud Native Security: 100%
- Mock exam (overall with 60 questions): 90%

Score by Domain on the overall mock exam:

- Cloud Native Security: 7 / 7 (100.0%)
- Compliance and Security Frameworks: 3 / 3 (100.0%)
- Kubernetes Cluster Component Security: 14 / 16 (87.5%)
- Kubernetes Security Fundamentals: 19 / 21 (90.5%)
- Kubernetes Threat Model: 8 / 9 (88.9%)
- Platform Security: 3 / 4 (75.0%)

### Questions to review (KCSA overall mock exam)

1. Which of the following actions in Kubernetes cross trust boundaries and potentially introduce security risks? (Select all that apply)

    > Your answer: `Mounting hostPath volumes into pods, Using service accounts across different namespaces`
    >
    > Correct answer: `Mounting hostPath volumes into pods, Using service accounts across different namespaces, Pulling container images from public registries`

    Explanation: Mounting hostPath volumes allows pods to access the host filesystem, crossing the boundary between container and host, which poses significant security risks. Using service accounts across namespaces can bypass namespace isolation, potentially escalating privileges. Pulling images from public registries introduces risks from untrusted or malicious images. Accessing the Kubernetes API is a controlled and authenticated action, while pod-to-pod communication within the same namespace is generally within the same trust boundary and less risky.

    > Domain: Kubernetes Threat Model

2. In Kubernetes, how can you apply an AppArmor profile to all containers within a Pod?

    > Your answer: `Annotate each container with the AppArmor profile`
    >
    > Correct answer: `Annotate each container with the AppArmor profile, Use the 'appArmorProfile' field in the container spec`

    Explanation: To apply an AppArmor profile to all containers in a Pod, you can use one of two methods: 1. **Annotations**: Use the annotation `container.apparmor.security.beta.kubernetes.io/<container_name>: <profile_ref>` for each container in the Pod. This method has been traditionally used and is backward-compatible. 2. **`appArmorProfile` field in `securityContext`**: Starting with Kubernetes v1.30, you can specify the AppArmor profile directly in the `securityContext` of a container using the `appArmorProfile` field. This method is more intuitive and avoids relying on annotations. Other options, such as setting it in the Deployment metadata or node-level configuration, are not valid methods for applying AppArmor profiles to specific containers.

    > Domain: Platform Security

3. In a Kubernetes NetworkPolicy specification, which field defines the allowed egress destinations for Pods?

    > Your answer: `to`
    >
    > Correct answer: `egress`

    Explanation: The 'egress' field in a NetworkPolicy spec specifies the rules that allow outbound traffic from selected Pods to specified destinations. The 'ingress' field defines allowed inbound traffic. 'to' is a subfield inside 'egress' rules specifying destination peers but is not the top-level field defining egress rules. 'from' is used in ingress rules to specify source peers. 'destinations' is not a valid field in NetworkPolicy.

    > Domain: Kubernetes Security Fundamentals

4. Select all that apply: Which Kubernetes object(s) allow you to define a set of rules for validating or mutating admission requests?

    > Your answer: `ValidatingWebhookConfiguration, MutatingWebhookConfiguration, AdmissionController`
    >
    > Correct answer: `ValidatingWebhookConfiguration, MutatingWebhookConfiguration`

    Explanation: `ValidatingWebhookConfiguration` and `MutatingWebhookConfiguration` are used to integrate admission webhooks that validate or mutate admission requests. Option 2 (`AdmissionController`) is a general term for the mechanism but not a specific Kubernetes object. Option 3 (`CustomResourceDefinition`) is used to define custom resources, and Option 4 (`WebhookPolicy`) is not a valid Kubernetes object.

    > Domain: Kubernetes Security Fundamentals

5. Which directories on a client machine contain sensitive information related to accessing Kubernetes clusters? (Select all that apply)

    > Your answer: `~/.kube/config`
    >
    > Correct answer: `~/.kube/config, ~/.ssh/`

    Explanation: The ~/.kube/config file stores Kubernetes cluster access credentials and configuration, making it sensitive. The ~/.ssh/ directory contains SSH keys used for secure access to nodes or clusters. Other directories like /var/log/, /etc/hosts, and /tmp/ do not typically contain sensitive Kubernetes access information.

    > Domain: Kubernetes Cluster Component Security

6. Which kubectl command displays the Kubernetes client and server versions for the cluster?

    > Your answer: `kubectl cluster-info`
    >
    > Correct answer: `kubectl version`

    Explanation: The command 'kubectl version' shows both the client and server (cluster) Kubernetes versions, which is useful for verifying compatibility and troubleshooting. Other commands like 'kubectl get version' or 'kubectl info' are invalid or do not provide version details. 'kubectl cluster-info' provides cluster endpoint information but not version details.

    > Domain: Kubernetes Cluster Component Security

## Day 8 (02 Jan 2026)

Review of previous questions and more general purpose mock exams.

### Mock exam (overall with 60 questions): 97%

Score by Domain on the overall mock exam:

- Cloud Native Security: 7 / 7 (100.0%)
- Compliance and Security Frameworks: 9 / 9 (100.0%)
- Kubernetes Cluster Component Security: 12 / 13 (92.3%)
- Kubernetes Security Fundamentals: 23 / 24 (95.8%)
- Kubernetes Threat Model: 2 / 2 (100.0%)
- Platform Security: 5 / 5 (100.0%)

### Questions to review (KCSA overall mock exam)

1. What is the default Kubernetes behavior when a container exceeds its configured memory limit?

    > Your answer: The container is throttled
    >
    > Correct answer: The container is terminated

    Explanation: When a container exceeds its memory limit, the Kubernetes kubelet kills the container to prevent it from affecting other workloads. This is known as an Out Of Memory (OOM) kill. The container is terminated and may be restarted depending on the pod's restart policy. CPU limits result in throttling, but memory limits cause termination. Kubernetes does not move pods automatically to other nodes or allocate additional memory beyond the specified limit.

    > Domain: Kubernetes Cluster Component Security

2. Why might a pod retain privileged access even after applying the 'restricted' Pod Security Standard (PSS) or Pod Security Admission (PSA) policy to its namespace?

    > Your answer: The Pod Security Standard does not apply retroactively to existing pods, The namespace labels required for policy enforcement are misconfigured or missing, The pod specification explicitly overrides the policy
    >
    > Correct answer: The Pod Security Standard does not apply retroactively to existing pods, The namespace labels required for policy enforcement are misconfigured or missing

    Explanation: The 'restricted' Pod Security Standard or Pod Security Admission policy applies only to pods created or updated after the policy is enforced; existing pods are not automatically remediated. Enforcement also depends on correct namespace labeling (e.g., 'pod-security.kubernetes.io/enforce=restricted'). If these labels are missing or misconfigured, the policy will not be enforced on pods in that namespace.

    > Domain: Kubernetes Security Fundamentals

Second mock exam (60 questions): 97%

### Second mock exam (60 questions): 97%

Score by Domain:

- Cloud Native Security: 7 / 7 (100.0%)
- Compliance and Security Frameworks: 5 / 5 (100.0%)
- Kubernetes Cluster Component Security: 18 / 18 (100.0%)
- Kubernetes Security Fundamentals: 14 / 16 (87.5%)
- Kubernetes Threat Model: 7 / 7 (100.0%)
- Platform Security: 7 / 7 (100.0%)

### Questions to review (Second mock exam)

1. Which field in the Pod spec can you use to disable privilege escalation for all containers in the Pod?

    > Your answer: `spec.securityContext.allowPrivilegeEscalation: false`
    >
    > Correct answer: `spec.containers.securityContext.allowPrivilegeEscalation: false`

    Explanation: The correct field to disable privilege escalation for all containers in a Pod is 'spec.containers.securityContext.allowPrivilegeEscalation: false'. This must be set individually for each container within the Pod, as there is no global 'allowPrivilegeEscalation' setting at the Pod level.

    > Domain: Kubernetes Security Fundamentals

2. Which of the following restrictions does the 'baseline' Pod Security Standard enforce? (Select all that apply)

    > Your answer: `Disallows privileged containers, Requires running as non-root`

    > Correct answer: `Disallows privileged containers, Blocks host networking and ports`

    Explanation: The 'baseline' Pod Security Standard disallows privileged containers and blocks host networking and ports. This ensures that pods are less vulnerable to security risks by limiting their capabilities.

    > Domain: Kubernetes Security Fundamentals

## Day 9 (03 Jan 2026)

Exam day. It was ok, a lot of questions that were trying to trick me.

Let's go to the CKS.
