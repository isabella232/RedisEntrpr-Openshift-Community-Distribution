# RedisEntrpr-Openshift-Community-Distribution [OKD](https://www.okd.io/)


1. Create a VM instance on GCP with following
  
Requirement  | Specification  
------------ | -------------
CPU | 8 (16 cores for "full" cluster)
Memory | at least 64 GB
OS | Cent OS 7
Disk | 100 GB

2. ssh to the machine

```bash 

sudo su
yum install -y git
sed -i 's/PermitRootLogin no/PermitRootLogin yes/' /etc/ssh/sshd_config
systemctl restart sshd
ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

3. Now ssh to public ip address and the private ip of the machine to make sure we have passwordless access. 

```bash
ssh {public ip }
exit
ssh {private ip }
exit
```

4. Clone the repo

```bash

git clone https://github.com/okd-community-install/installcentos

```

5. Set the env varaible. Replace the public ip. Accepts all the defaults from install-openshift.sh.

```bash
cd installcentos/

export DOMAIN={public Ip}.nip.io
export USERNAME=admin
export PASSWORD=admin

./install-openshift.sh

```

Note: Installtion takes time... (45-60 minutes is normal)

6. Access the UI from https://console.{Public IP}.nip.io:8443


7. Install Redis Enterprise. Based on [redis-enterprise-k8s-docs](https://github.com/RedisLabs/redis-enterprise-k8s-docs#configuration)

```bash

cd 
git clone https://github.com/Redislabs-Solution-Architects/RedisEntrpr-Openshift-Community-Distribution.git
cd RedisEntrpr-Openshift-Community-Distribution
oc new-project {my-project}
oc apply -f scc.yaml
oc adm policy add-scc-to-group redis-enterprise-scc system:serviceaccounts:{my-project}
kubectl apply -f bundle.yaml
kubectl apply -f redis-storage-class.yaml
kubectl apply -f redis-storage.yaml 
kubectl apply -f redis-enterprise-cluster.yaml
```

8. Setting up route to Redis UI

![Route 1](/images/route1.png)

![Route 2](/images/route2.png)

 You can access the Redis UI from Hostname URL
 
![Route 3](/images/route3.png)


9. Find the Redis User and Password

![secret 1](/images/secret1.png)

![secret 2](/images/secret2.png)

![secret 3](/images/secret3.png)


