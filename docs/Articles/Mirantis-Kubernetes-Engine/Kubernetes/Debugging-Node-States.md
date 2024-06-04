# Debugging Node States in Kubernetes

## Possible Node States

Nodes run pods and therefore must be maintained to have a healthy cluster. A node could be a physical machine or run as a virtualized system.

We can run `kubectl get nodes` within a cluster to check on the health of nodes:

```bash
$ kubectl get nodes
NAME           STATUS     ROLES    AGE     VERSION
node1          Ready      <none>   1d      v1.21.4
node2          Ready      <none>   1d      v1.21.4
node3          Ready      <none>   1d      v1.21.4
node4          NotReady   <none>   1d      v1.21.4
node5          NotReady   <none>   1d      v1.21.4
node6          Ready      <none>   1d      v1.21.4
```

A node can report any of the following states:

* `Ready`
    * This means the node is healthy and ready to accept pods.
* `NotReady`
    * This means the node has encountered an issue and cannot schedule new pods to it.
* `SchedulingDisabled`
    * This means the node has been marked as unavailable to be scheduled to. Usually this is managed by the `kubectl cordon` command.
* `Unknown`
    * This means the node is not reachable from the control plane and further status cannot be determined.

## What do I do when my node enters "NotReady" state?

Let's explore the possible reasons a node would report as "NotReady":

- Limited Resources
- Incorrectly Configured Network
- kubelet Process Error
- kube-proxy Communication Error
- Cloud Specific

Here are some generic guidelines on how to approach diagnosising problems with your nodes:

1. Inspect the kube-proxy pod:

    - We want to make sure there is only 1 kube-proxy pod per node running:

    ```
    kubectl get pods -n kube-system -o wide
    ```

    ```
    NAME                               READY   STATUS    RESTARTS   AGE   IP            NODE      NOMINATED NODE   READINESS GATES
    coredns-74ff55c5b-k8mjr            1/1     Running   0          1d    10.244.0.2    node1     <none>           <none>
    coredns-74ff55c5b-q7rqm            1/1     Running   0          1d    10.244.0.3    node1     <none>           <none>
    etcd-node1                         1/1     Running   0          1d    192.168.1.1   node1     <none>           <none>
    kube-apiserver-node1               1/1     Running   0          1d    192.168.1.1   node1     <none>           <none>
    kube-controller-manager-node1      1/1     Running   0          1d    192.168.1.1   node1     <none>           <none>
    kube-proxy-5h2k8                   1/1     Running   0          1d    192.168.1.1   node1     <none>           <none>
    kube-scheduler-node1               1/1     Running   0          1d    192.168.1.1   node1     <none>           <none>
    ```

2. Next, inspect any nodes where the `kube-proxy` pod is not `Running`:

    ```bash
    kubectl describe pod kube-proxy-5h2k8 -n kube-system
    ```

3. Access the logs of the pod to further delineate between events for determining potential causes:

    ```bash
    kubectl logs kube-proxy-5h2k8 -n kube-system
    ```

4. For the case where a node does not have the `kube-proxy` pod `Running`, inspect the daemonset:

    ```bash
    kubectl describe daemonset kube-proxy -n kube-system
    ```

5. Describe the node too. We want to know details about any nodes that report `NotReady`:

    ``` bash
    kubectl describe node <nodeName>
    ```

    !!! info

        Here, the `Conditions` section is particularly useful to determine if the node is running low on resources:

        - If `MemoryPressure` is true, the node is running out of memory. Inspect processes and other pods to see what is consuming.
        - If `DiskPressure` is true, the node is running out of virtual or physical hard disk space. Use command line tools like `du` and `mount` to determine usage.
        - If `PIDPressure` is true, the node is handling too many processes at once.
        - If `NetworkUnavailable` is true, further inspect the node's network to determine where the configuration is incorrect.
        - If `Ready`, the node is reporting healthy and ready to accept new pods. A false is equivalent to `NotReady` state. An `Unknown` here means the node controller has not heard back from the node in the past `node-monitor-grace-period` (default is 40 seconds).

6. Let's also verify that kubelet is running:

    If the previous describe returned `Unknown` for all the conditions, then it is probably an issue with `kubelet`.

    Let's inspect it on the node with:

    ``` bash
    systemctl status kubelet
    ```

    Service should be `Active` and running. `Inactive` means the process has stopped.

    Further inspect causes of the process crash with `journalctl -u kubelet` and finally restart the process: `systemctl restart kubelet`

7. What if the Network is Unavailable?

    Ask the following questions:

    - Does the node use a proxy? Does the proxy allow access to the API server endpoints?
    - Are the routing tables configured to allow communication with the API server endpoints?
    - Is the node cloud hosted (like AWS)? If so, could the VPC network rules be blocking communication between the control plane and node?

    Verify the node can reach the API server endpoints with the following command:

    ```bash
    nc -vz <api-server-endpoint> 443
    ```

    !!! tip

        Running a netcat command might require spinning up another container on the problem node. I would recommend using [Netshoot](https://github.com/nicolaka/netshoot) and run the container with:
        ```bash
        kubectl debug mypod -it --image=nicolaka/netshoot
        ```
