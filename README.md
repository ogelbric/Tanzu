# TANZU
VMware vCenter 7.0 update 1 Guest cluster depoloyment with HAproxy as load balancer

**The base is William Lams power shell script (https://github.com/lamw/vsphere-with-tanzu-basic-automated-lab-deployment)**


**The nested network layout:**

```
192.168.4.x  Gateway: 192.168.4.10
192.168.6.x  Gateway: 192.168.6.10
DNS 10.118.87.50
NTP 10.128.152.81

HAproxy IP 192.168.4.2 / 192.168.6.2
Ingress 192.168.6.128/26
```




