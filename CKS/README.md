# The journey to CKS certification

## Day 1 (20 Dec 2025)

With the difficulty of the CKS exam, I will study for it in conjunction with the KCSA material, addicionally, starting with hands-on scenarios, following the tips of @mischavandenburg in his youtube video: [CKS Study Guide 2024](https://www.youtube.com/watch?v=_l232KiJHNA).

- Review of CKS covered topics
- Hands-on scenarios at killercoda.com:
  - Apiserver Crash
  - Apiserver Misconfigured

## Day 2 (30 Dec 2025)

Started the CKS course on youtube by @brancz: [CKS - Killer Shell Course on youtube](https://www.youtube.com/watch?v=d9xfB5qaOfg).

- Hands-on scenarios at killercoda.com:
  - Container namespaces Docker
  - Container namespaces Podman

- Killer Shell CKS course: 10% completed, 90% to go.

## Day 3 (31 Dec 2025)

Continued the CKS course on youtube.

- Hands-on scenarios at killercoda.com:
  - NetworkPolicy Create Default Deny
  - NetworkPolicy Metadata Protection
  - NetworkPolicy Namespace Selector
  - Ingress Create
  - Ingress Secure
  - CIS Benchmarks fix Controlplane
  - Verify Platform Binaries
  - RBAC ServiceAccount Permissions
  - RBAC User Permissions
  - ServiceAccount Token Mounting
  - CertificateSigningRequest sign manually
  - CertificateSigningRequest sign via API
  - Apiserver NodeRestriction

- Killer Shell CKS course: 45% completed, 55% to go.

## Day 4 (01 Jan 2026)

Continued the CKS course on youtube.

- Hands-on scenarios at killercoda.com:
  - Secret ETCD Encryption
  - Secret Access in Pods
  - Secret Read and Decode
  - Secret ServiceAccount Pod
  - Sandbox gVisor

- Killer Shell CKS course: 58% completed, 42% to go.

## Day 5 (02 Jan 2026)

Continued the CKS course on youtube.

- Hands-on scenarios at killercoda.com:
  - Privilege Escalation Containers
  - Privileged Containers
  - Container Hardening
  - Container Image Footprint User
  - Immutability Readonly Filesystem

- Killer Shell CKS course: 71% completed, 29% to go.

## Day 6 (03 Jan 2026)

Continued the CKS course on youtube.

- Hands-on scenarios at killercoda.com:
  - Static Manual Analysis Docker
  - Static Manual Analysis K8S
  - Image Vulnerability Scanning Trivy
  - Image Use Digest
  - ImagePolicyWebhook Setup

- Killer Shell CKS course: 85% completed, 15% to go.

## Day 7 (04 Jan 2026)

Finished the CKS course on youtube and started the Kodekloud CKS course.

- Hands-on scenarios at killercoda.com:
  - Syscall Activity Strace
  - Falco Change Rule
  - Auditing Enable Audit Logging
  - AppArmor
  - System Hardening Close Open Ports
  - System Hardening Manage Packages

- Killer Shell CKS course: 100% completed, 0% to go.
- Kodekloud CKS course: 10% completed, 90% to go.

## Day 8 (05 Jan 2026)

Continued the CKS course at Kodekloud.

- Kodekloud CKS course: 20% completed, 80% to go.

## Day 9 (06 Jan 2026)

Continued the CKS course at Kodekloud. Finished the review of Cluster Setup and Hardening and System Hardening (Linux) topics

- Kodekloud CKS course: 54% completed, 46% to go.

## Day 10 (07 Jan 2026)

Continued the CKS course at Kodekloud. Review of Minimize Microservices Vulnerabilities topics.

- Kodekloud CKS course: 72% completed, 28% to go.

## Day 11 (08 Jan 2026)

Continued the CKS course at Kodekloud. Finished the review of Minimize Microservices Vulnerabilities topics.

- Kodekloud CKS course: 90% completed, 10% to go.

## Day 12 (09 Jan 2026)

Continued the CKS course at Kodekloud. Finished the review of Monitoring, Logging and Runtime Security topics.

- Mock exam 1: Impossible to trust the results at kodekloud, they were checking some requirements that were not described in the question.
- Mock exam 2: Started the first attempt of the Exam Simulator provided by Linux Foundation (killer.sh, CKS Simulator A)
  - 50/73 = 68.49% 😥

- Kodekloud CKS course: 100% completed, 0% to go.

### To review

- [x] `--kubernetes-service-node-port` flag should be disabled, it should be the same as changing the kubernetes service to ClusterIP?
  - Maybe, if the question asks to disable the node port, change both, the kubernetes service and the parameter on `kube-apiserver.yaml` file.
- [x] wtf is `Deployment has token-lifetime annotation`? Is it just literally an annotation with no purpose?
  - Do everything that the question asks, literally, pay attetion to every detail.
- [x] I don't know if it was a bug, but the `/etc/docker/daemon.json` file was not accepting the `"icc": false` parameter, I needed to add it to the systemd service file and after, including it again on the daemon.json file to make it work.
  - `dockerd --validate --config-file /etc/docker/daemon.json` to validate the file (it was ok, but it's always a good practice to validate)
- [x] appArmor question, missed one detail: Single container named c1 with the AppArmor profile **enabled only for this container**. I enabled the profile on the pod level, but it should be on the container level.
- [x] Question to update a immutable secret and update the pods that are using it. I delete all pods, but for some reason the environment variable was not updated. I forgot to just `k exec pod` and check the environment variable.
- [x] CiliumNetorkPolicy, missed one detail: `Allow egress to Endpoints in the same Namespace`, I thought it was just an `- toEndpoints: [{}]` to allow it, just as the [documentation](https://docs.cilium.io/en/stable/security/policy/language/#egress-allow-all-endpoints) says. But it was necessary to include the `matchLabels` too with `k8s:io.kubernetes.pod.namespace: <namespace>`, or the lab has a bug, I don't know. An important thing was the missing documentation around the `egressDeny`.
- [x] Questions related to falco: Read all the topics that the question asks, if you need to create some rule with conditions that were not explicitly described, make sure that you know what you are doing, don't lose time searching in the documentation.
- [x] Audit logs on kubernetes, if I didn't missed time on the falco question, I would have enought time to answer this question.

After the review, if I didn't missed the basic details (still disconsidering the falco question), I would have a score around 92%.

## Day 13 (10 Jan 2026)

- Mock exam 3: Second lab of Kodekloud, still some bugs on the evaluation of the score, but I was able to answer all questions, I think I would have a score around 80%.
- Mock exam 4: Killer.sh retry for the CKS Simulator A. Score of 67/73 = 91,78%.

### To review

- [x] Just an observation around audit policy, I need to pay attention on the details like: `Changes to deployments in the default namespace at the Request level`, this means that only verbs that changes the deployment should be audited, like: `create, update, patch, delete`.
- [x] Copy and paste every change that they ask (missed some points 'cause I included the image `alpine: 3.2` instead of `alpine: 3.12`)
- [x] Pay attention to what they ask around network policies, missed some points 'cause I included an `egress` instead of an `egressDeny`.
- [x] Pay attention to CiliumNetworkPolicy specs, do not repeat the fields `egress`, `egressDeny`, `ingress`, `ingressDeny`, etc. Again, to allow traffic in the same namespace just use `toEndpoints: [{}]`
