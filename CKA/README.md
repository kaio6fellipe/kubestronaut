# The journey to CKA certification

## Day 1 (11 Nov 2024)

Kodekloud is a great platform to learn and practice. As soon as I started my journey, I finished the Core Concepts in the first day (with practice tests), not a big deal 'cause I've been working with Kubernetes for a while now. Following up the kodekloud repository too: https://github.com/kodekloudhub/certified-kubernetes-administrator-course.

- 17% completed, 83% to go.

## Day 2 (12 Nov 2024)

Deep dive into Scheduling. Manual Scheduling (setting the node name on the pod), Labels and Selectors, Taints and Tolerations, Node Selectors, Node Affinity, Resource Limits, DaemonSets, Static Pods (omg :mindblowing:), Multiple Schedulers and Scheduling Profiles. Also finished the Logging & Monitoring section. Pretty basic, right to the point used in the exam.

Also finished the Application Lifecycle Management section, no surprises around that. A tip to get used to kubectl operations and yaml manifests structure is to always use the `kubectl [command] --help` and the official documentation, get used to it. For me, the best section around the documentation (until now) is the Kubernetes API reference: https://kubernetes.io/docs/reference/kubernetes-api/.

- 42% completed, 58% to go.

## Day 3 (13 Nov 2024)

Finished the Cluster Maintenance section. This section is very important to understand how to maintain a Kubernetes cluster, how to upgrade it, how to backup and restore, how to troubleshoot it, etc. Everything that a Cloud Provider does for you.

- 49% completed, 51% to go.

## Day 4 (14 Nov 2024)

Security section, deep dive in TLS.

- 54% completed, 46% to go.

## Day 5 (15 Nov 2024)

Finished Security section, Authentication, Authorization, Network Policies, Service Accounts, Security Contexts, etc.

- 65% completed, 35% to go.

## Day 6 (16 Nov 2024)

Finished Storage section, Volumes, Persistent Volumes, Storage Classes, StatefulSets, etc. Started and finished Networking section, Services, Ingress, Network Policies, etc.

- 85% completed, 15% to go.

## Day 7 (18 Nov 2024)

Design and Installation of a Kubernetes Cluster section. ETCD in HA. High Availability for K8S control plane.

- 88% completed, 12% to go.

## Day 8 (16 Dez 2024)

Kubernetes installation with kubeadm and troubleshooting for apps, control plane and worker nodes.

- 93% completed, 7% to go.

## Day 9 (17 Dez 2024)

Finished the course, now it's time to practice with the mock exams.

- Lightning Lab 1: 35% (1h)
  - Need to review: jsonpath and cluster upgrade (got stuck with kubeadm upgrade problem at one node)

## Day 10 (18 Dez 2024)

- Content review and re-do the Lightning Lab 1: 100% (1h)

## Day 11 (22 Dez 2024)

- Mock Exam 1: 92% (1h)
  - Pay attention to the task definition, read it carefully, missed a question because it asked to create a static pod on the **controlplane** node (`/etc/kubernetes/manifests/` folder) and I created with the Kubernetes API.

## Day 12 (23 Dez 2024)

- Mock Exam 2: 70% (1h)
  - Need to review CSR process and cluster DNS for pods.
- Mock Exam 3: 74% (1h)
  - Need to review advanced jsonpath filtering and network policies (mismached the `from` field and included an unused `podSelector` field).
