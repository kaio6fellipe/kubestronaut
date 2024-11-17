
# Tips for the CKA Exam

## 1. Use the `kubectl run` command to generate YAML files

Creating and editing YAML files is a bit difficult, especially in the CLI. During the exam, you might find it difficult to copy and paste YAML files from the browser to the terminal. Using the `kubectl run` command can help in generating a YAML template. And sometimes, you can even get away with just the `kubectl run` command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with a specific name and image, you can simply run the `kubectl run` command.

Use the below set of commands and try the previous practice tests again, but this time, try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises.

Reference (Bookmark this page for the exam. It will be very handy):

https://kubernetes.io/docs/reference/kubectl/conventions/

- Create an NGINX Pod

```bash
kubectl run nginx --image=nginx
```

- Generate POD Manifest YAML file (-o yaml). Don’t create it(–dry-run)

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

- Create a deployment

```bash
kubectl create deployment --image=nginx nginx
```

- Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run)

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

- Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

- Make necessary changes to the file (for example, adding more replicas) and then create the deployment.

```bash
kubectl create -f nginx-deployment.yaml
```

OR

In k8s version 1.19+, we can specify the –replicas option to create a deployment with 4 replicas.

```bash
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

## 2. Use the Imperative approach to save time

While you would be working mostly the declarative way – using definition files, imperative commands can help in getting one-time tasks done quickly, as well as generate a definition template easily. This would help save a considerable amount of time during your exams.

Before we begin, familiarize yourself with the two options that can come in handy while working with the below commands:

`--dry-run` By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the `--dry-run=client` option. This will not create the resource; instead, it tells you whether the resource can be created and if your command is right.

`-o yaml` This will output the resource definition in YAML format on the screen.

Use the above two in combination to generate a resource definition file quickly that you can then modify and create resources as required instead of creating the files from scratch.

### POD

- Create an NGINX Pod

```bash
kubectl run nginx --image=nginx
```

- Generate POD Manifest YAML file (-o yaml). Don’t create it(–dry-run)

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

### Deployment

- Create a deployment

```bash
kubectl create deployment --image=nginx nginx
```

- Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run)

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

- Generate Deployment with 4 Replicas

```bash
kubectl create deployment nginx --image=nginx --replicas=4
```

- You can also scale a deployment using the `kubectl scale` command.

```bash
kubectl scale deployment nginx--replicas=4
```

- Another way to do this is to save the YAML definition to a file and modify

```bash
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

You can then update the YAML file with the replicas or any other field before creating the deployment.

### Service

- Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

```bash
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

(This will automatically use the pod’s labels as selectors)

Or

```bash
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
```

(This will not use the pods labels as selectors, instead, it will assume selectors as app=redis.

You cannot pass in selectors as an option.

So, it does not work very well if your pod has a different label set. So, generate the file and modify the selectors before creating the service)

- Create a Service named nginx of type NodePort to expose pod nginx’s port 80 on port 30080 on the nodes:

```bash
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```

(This will automatically use the pod’s labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port manually before creating the service with the pod.)

Or

```bash
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

(This will not use the pod labels as selectors.)

Both the above commands have their own challenges. While one of them cannot accept a selector, the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

Reference:
https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

https://kubernetes.io/docs/reference/kubectl/conventions/

## 3. Around Logging and Monitoring

- Just understand how to use the `kubectl logs` and `kubectl top`(depends on metrics-server) commands. Don't worry about complex monitoring solutions for the exam.

## 4. When in doubt, double check

- This applys to everything in the exam. You must verify your work by yourself. For example, if the question is to create a pod with a specific image, you must run the kubectl describe pod command to verify the pod is created with the correct name and correct image.

## 5. Make sure that you really understand TLS

- Understand how to generate certificates, how to use them in Kubernetes, how to secure the communication between the components, etc. Certificate Health Check Spreadsheet provided by kodekloud: https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/tools/kubernetes-certs-checker.xlsx

To check certificate information:
```bash
openssl x509 -in file.crt -text -noout
```

In case of problems, always check the logs of the component that is having the problem. Since the components are responsible for the health of the cluster, you will not be able to see the logs with kubectl logs, you will need to check the logs in the node where the component is running (generally in the controlplane).

Check for containers in execution (if the component is running as a static pod):
```bash
crictl ps -a
```

Check for logs of a container:
```bash
crictl logs [container-id]
```

If the components are running as a Linux service in the node (generally provisioned without kubeadm), you can check the logs with journalctl:
```bash
journalctl -u [service-name]
```

### TLS Examples

Every client/server in Kubernetes has a certificate, depending of the communication between the components. The kube-apiserver has a certificate, the kubelet has a certificate, the kube-scheduler has a certificate, etc.

#### Certificate Authority (used to sign other certificates)

1. CA key

```bash
openssl genrsa -out ca.key 2048
```

2. CA csr

```bash
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
```

3. CA crt

```bash
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```

#### Admin User Certificate

1. Admin key

```bash
openssl genrsa -out admin.key 2048
```

2. Admin csr

```bash
openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
```

3. Admin crt

```bash
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
```

Using the certificates to authenticate with the kube-apiserver:

```bash
curl https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt
```

## 6. Make sure that you really understand Networking on Kubernetes

- Understand networking concepts, how it works on Linux Systems, how it works on namespaced environments and how it works on Kubernetes.
