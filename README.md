Ansible Playbook: Deploy a Kubernetes cluster
=============================================

# Requirements
 * At least three servers running Centos8 (1x `kube_master` and 2x `kube_workers`).
 * All machines have their hosts file updated to include the hostnames of all other machines.
 * The Ansible host-patterns are configured as:
   * `[kube_nodes]` - This will contain all the Kubernetes nodes (master and workers).
   * `[kube_workers]` - This will contain the Kubernetes worker nodes.
   * `[kube_master]` - This will contain the Kubernetes master node(s).

# Post deployment
On the master node, run the command (as root) `kubeadm init --pod-network-cidr=10.244.0.0/16`, from there, you _should_ be given the token for the worker nodes to join the cluster.

To add a pod network (here I am using Flanel), run the command (as root) `kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`.

You can set your nodes to the worker node role using the command: `for i in {1..4}; do kubectl label node kube0$i node-role.kubernetes.io/worker=worker; done` 
