# k3s-ansible

This codes provisions an HA K3S Cluster on AWS EC2 Instances. The cluster runs three ETCD Masters and three Workers. 

**Prerequisites**

1. Acees Key ID and Secret Access Key for an IAM user with permissions to create instances
3. ssh Key Pair to access instances
4. A VPC subnet ID
5. A Security Group Name with the K3S TCP and UDP Ports added to the Security Group
6. Ansible > 2.9.6 and AWS SDK for Python (Boto3) 

**Usage**

Update the values in vars/keys_master.yml and vars/keys_worker.yml. For example, to deploy the Masters on AWS free-tier in us-east-1 add the below to vars/keys_master.yml:

```python
AWS_ACCESS_KEY_ID: 12345678910
AWS_SECRET_ACCESS_KEY: ABCDEFGHIJ
AWS_REGION: us-east-1
INSTANCE_TYPE: t2.micro
IMAGE: ami-038f1ca1bd58a5790
VPC_SUBNET_ID: subnet-12345678910
GROUP: k3s-security-group01
TAG: Master_Node
K3S_TOKEN: my-secret-token
SSH_KEYPAIR_NAME: keypair01
SSH_KEY: /home/ansible/keypair01.pem
```
Run the Playbook to start the install. 
```python
ansible-playbook k3s.yml
```
The Public IP's for the Master and Worker Nodes will be printed to screen. Once the Playbook has completed, you can ssh to the Master Nodes and run 'kubectl get nodes' to ensure all Nodes are READY.

The test-nginx.yml can be used to confirm access outside of the the Cluster. Copy (scp) the test-nginx.yml to one of your Master Nodes then apply it 
```python
kubectl apply -f test-nginx.yaml
```

Enter the IP adderess of your Nodes into a browser to view the Welcome to nginx page.
