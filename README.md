# Setup Instructions

Follow these steps to create setup k8 clusters:

- STEP1: create 2 vms or run 2 aws instances.
- STEP2: 1 vm call it master and other call in worker
- STEP3: on master vm run k8s-master.sh
- STEP4: on worker vm run k8s-worker.sh
- STEP5: switch to master vm and get the join token using following command
 ```sh
           kubeadm token create --print-join-command
```
- STEP6: now switch back to worker vm join master with the token link by running the output of above command on terminal e.g

```
kubeadm join 172.16.56.137:6443 --token qy8pv1.g9hz6f4t6j75lxnc \
	--discovery-token-ca-cert-hash sha256:3e3677c30cdd5d9a9f066ecf1d47e928addad5570c6485e04fd1f218fae39811 
```

- STEP7: Verify that worker joined successfuly the cluster by navigating to your master server using following command 
```
kubectl get nodes
```

![image](https://user-images.githubusercontent.com/10846423/235523675-c5ca69d5-bbc5-4bd6-80d1-ee10515b5ea8.png)

If you see above output, it confirms that master and worker nodes are in ready status. Now, this cluster is ready for the workload.


---

> **Note:** if during setup you didn't changed the **pod network CIDR** to a different ip range then you don't need to do updated it in the custom-resources.yaml. However, if you initialized it with different ip range e.g 10.0.0.0/16 using follwoing command:
```
sudo kubeadm init --pod-network-cidr=10.0.0.0/16 
```
then you will need to updated it in the custom-resources.yaml file as well using below command:
```
sed -i 's/cidr: 192\.168\.0\.0\/16/cidr: 10.0.0.0\/16/g' custom-resources.yaml
```
---
