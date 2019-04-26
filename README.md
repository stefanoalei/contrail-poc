# Contrail POC

# 1 Overview

Pick cluster and underlay to build the POC.

### Underlay
* [Bridge](underlay/bridge.md)
  * Contrail integration deployment.
  * Contrail basic features.
* [Gateway MX](underlay/gateway-mx.md)
  * Overlay-underlay connectivity via MX.
  * Distributed fabric forwarding (gateway-less mode).
* [IP Fabric](underlay/fabric.md)
  * Contrail Fabric Management
* [IP Fabric HA](underlay/fabric-ha.md)
  * Contrail Fabric Management HA
* [Gateway to Gateway](underlay/gateway-gateway.md)
  * Contrail gateway to corp gateway.
  * Contrail gateway to Contrail gateway (DC-DC).
  * Remote compute.


### Cluster
* [Contrail and Openstack](cluster/openstack/openstack.md)
* [Contrail and Openstack HA](cluster/openstack-ha/openstack-ha.md)
* [Contrail and Kubernetes](cluster/kubernetes/kubernetes.md)
* [Contrail and Kubernetes HA](cluster/kubernetes-ha/kubernetes-ha.md)
* [Contrail and OpenShift Origin](cluster/openshift-origin/openshift-origin.md)
* [Contrail Fabric Management](cluster/cfm/cfm.md)


# 2 Hypervisor

Here are prerequisites for the hypervisor.

* Resource
  * The hypervisor used by this guide has 32 vCPU, 256GB memory and 2TB disk.
  * At least one NIC that has public/internet access.

* Host OS
  * CentOS 7.5 is validated.

* Disk partition
  * Use logical volume.
  * Make single root `/` LV, don't create separate home `/home` LV.
  * For better disk performance, reserve a partition for libvirt volume.

* Kernel
  * Upgrade kernel and reboot, after installation.


# 3 Setup hypervisor

* Install `git`.
```
yum install git
```

* Clone repository.
```
git clone https://github.com/tonyliu0592/contrail-poc.git
```

* Update the followings variables in `poc.conf`.
  * `volume_dev`
  * `ntp_server`

* Setup the hypervisor.
```
cd contrail-virtual-fabric
poc setup-hypervisor
```

* Copy `vm-image` to `/opt` directory.
```
CentOS-7-x86_64-GenericCloud-1805.qcow2
cirros-0.4.0-x86_64-disk.img
vmx-re.qcow2
vmx-re-hdd.qcow2
metadata-usb-re.img
vFPC-20180829.img
vqfx-pfe.qcow2
vqfx-re.qcow2
```


# 4 Deploy

## 4.1 Build underlay and cluster

* Update variables in `poc.conf` for deployment.
  * Choose underlay and cluster.
  * Set username and password for hub.juniper.net/contrail.
  * Set Contrail container tag and Contrail Command container tag.
  * Set Ansible playbook name.

* Build underlay and cluster.
```
poc build-underlay
poc build-cluster
```


## 4.2 Change underlay

* Delete existing underlay.
```
poc delete-underlay
```
#### Note: Delete underlay before updating `poc.conf`.

* Update underlay selection in `poc.conf`.

* Build new underlay.
```
poc build-underlay
```


## 4.3 Change cluster

* Delete existing cluster.
```
poc delete-cluster
```
#### Note: Delete cluster before updating `poc.conf`.

* Update cluster selection in `poc.conf`.

* Build new cluster.
```
poc build-cluster
```
