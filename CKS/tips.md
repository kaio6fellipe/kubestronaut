
# Tips for the CKS Exam

> [Handbook](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2) and [Important instructions](https://docs.linuxfoundation.org/tc-docs/certification/important-instructions-cks)

I saw everyone talking about the CKS exam being way harder than the CKA and CKAD, so, because of that, I will spend more time on hands-on labs, following some tips provided by [@mischavandenburg](https://github.com/mischavandenburg) in his youtube video: [CKS Study Guide 2024](https://www.youtube.com/watch?v=_l232KiJHNA), which would be:

- [CKS - Killer Shell Course on youtube](https://www.youtube.com/watch?v=d9xfB5qaOfg)
- [Killer Shell CKS Simulators (also RBAC and Network policies from others)](https://killercoda.com/)
- [killer.sh](https://killer.sh/cks)
zettelkasten.mischavanderburg.net

Additionally, almost the same tips from CKA and CKAD too:

- Get confortable with `kubectl`, use aliases if needed (but don't be obsessed about it, personally I used only `k` for `kubectl`).
- You're not alone in the exam, you can use the browser within the VM to access the Kubernetes documentation: https://kubernetes.io/docs/ or https://kubernetes.io/blog.
- Review your work, always double check your answers.
- Make sure to copy and paste the names correctly from the question, it's easy to make mistakes.
- Manage your time effectively, don't spend too much time on a single question, you don't have to get everything right to pass the exam.

## Kubelet information gathering and important infos

1. Check how the kubelet is running and identity the configurations used on the parameters (`--config` is the kubelet configuration file)

    ```bash
    ps aux | grep kubelet
    ```

2. Kubelet configs, ports, authentication and authorization:

   - port `10250`: API with full access
   - port `10255`: Read only API
   - `authentication`: enable `anonymous` access or not, `x509` (client certificates), `webhook` (bearer token)
   - `authorization`: mode on how the user will be authorized on API calls, `Webhook` means that the authorization will be delegated to the api server, `AlwaysAllow` is always allow all requests (default).
   - `readOnlyPort`: port for the read only API, default is `10255`, set to `0` to disable it.

## Securing Container Runtime

- Run containers with least privileges (non-root users/groups on `spec.securityContext.runAsUser` and `spec.securityContext.runAsGroup`).
- Use read-only root filesystem (set `spec.securityContext.readOnlyRootFilesystem` to false).
- Set container limits to prevent resource exhaustion on the host node.
- Use security profiles (`spec.containers[].securityContext.seLinuxOptions` and `metadata.annotations.container\.apparmor\.security\.beta\.Kubernetes\.io/my-profile`). This would limit the actions that a container can perform.

## Securing kube-proxy

- `ps aux | grep kube-proxy` to check how the kube-proxy is running and identity the configurations used on the parameters (`--config` is the kube-proxy configuration file).
- The kube-proxy config file should have a minimum permissions of at least `644` (read and write for the user and group). And the owner should be the user running the kube-proxy (`root:root`).
- Communication between kube-proxy and API server should be encrypted using TLS. (Usually set on the kubeconfig file, eg: `/var/lib/kube-proxy/kubeconfig.conf`)
- Audit logs should be enabled for kube-proxy service. Usuarlly located with the command `/var/log/audit/audit.log | jq .objectRef.resource`.

## Pod security

- Avoid the usage of privileged containers. Use the `securityContext.privileged` field to set the privilege level of the container.
- `securityContext.runAsUser` and `securityContext.runAsGroup` should be set to a non-root user and group.
- Avoid capabilities like `CAP_SYS_BOOT`.
- Avoid `hostPath` mounts.
- Use Pod Security Admission (PSA) or Pod Security Standards (PSS). The Pod Security Policy (PSP) was removed at version `1.25`. PSA should be enabled on the API server under the flag `--enable-admission-plugins=PodSecurityPolicy`.
  - How it works: 
    ```
    kubectl -> authentication -> authorization -> admission controlers (PodSecurityPolicy) -> create a pod
    ```

## Securing etcd

- Data at rest should be encrypted.
- Add the `--encryption-provider-config` flag to the etcd server to enable encryption at rest and point to the encryption config file.
- Secure data in transit with TLS. The flag `--client-cert-auth` enable client certificate authentication.
- Etcd backups is important
  ```bash
  ETCDCTL_API=3 etcdctl snapshot save /path/to/backup.db --endpoints=<etcd-endpoints> --cacert=/path/to/ca.crt --cert=/path/to/etcd-client.crt --key=/path/to/etcd-client.key
  ```
- Restore etcd backups
  ```bash
  ETCDCTL_API=3 etcdctl snapshot restore /path/to/backup.db --endpoints=<etcd-endpoints> --cacert=/path/to/ca.crt --cert=/path/to/etcd-client.crt --key=/path/to/etcd-client.key --data-dir=/path/to/etcd-data
  ```

## Container Networking Security

- Use Network Policies to restrict traffic between pods. By default kubernetes allows all traffic between pods.
- Using Service Mesh like Istio or Linkerd for MTLS
- To encrypt network traffic between nodes we can use WireGuard or IPSec, Calico is an option too.
  - Cilium provides those features too.
- Segregate workloads in different namespaces.

## Securing storage

- Encrypt data at rest, when using a StorageClass, set the `parameters.encrypted` field to `true`, for example when using EBS on AWS.
- Access control with RBAC, what resources can access PVCs
- Backup and Disaster Recovery, with tools like Velero, portworx, OpenEBS, veeam, etc.

## Client Security

### Using `kube proxy` command

- Send requests to the API server with all configurations sets
    ```bash
    kubectl proxy
    ```

    ```bash
    curl -k http://localhost:8001
    ```

- Send requests to any service in the cluster (not exposed through NodePort or LoadBalancer)

    ```bash
    curl -k http://localhost:8001/api/v1/namespaces/<namespace>/services/<service-name>/proxy/
    ```

- Configuring a port forward

    ```bash
    kubectl port-forward service/nginx <local-port>:<service-port>
    ```

    ```bash
    curl http://localhost:<local-port>
    ```

### kubeconfig

- By default the kubectl will search for a kubeconfig file at `~/.kube/config`.
- The kubeconfig file has the following sections:
  - Clusters
    - List of clusters with server address and certificate authority location or in base64 format.
  - Users
    - List of users with name and client certificate and key.
  - Contexts
    - Which user account will be used to access the cluster
- The config `current-context` is the context that will be used to access the cluster.
  - To change the context, use the command `kubectl config use-context <context-name>`.
