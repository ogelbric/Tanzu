# TANZU
VMware vCenter 7.0 update 1 Guest cluster depoloyment with HAproxy as load balancer

**The base is William Lams power shell script (https://github.com/lamw/vsphere-with-tanzu-basic-automated-lab-deployment)**


**The nested network layout:**

```
192.168.4.x  Gateway: 192.168.4.1
192.168.6.x  Gateway: 192.168.6.1
DNS 10.118.87.50
NTP 10.128.152.81

HAproxy IP 192.168.4.2 / 192.168.6.2
Ingress 192.168.6.128/26
```

**The script with 192.168.4.x and 192.168.6.x ranges and using 2 datastores (ESXi / vCenter and HAproxy)**

![GitHub](vsphere_basic_with_HA_Proxy(96GWat1).ps1)

**The following DNS have to made (reverse DNS!)** 

```
tanzu-esxi-2	Host (A)	192.168.4.52
tanzu-esxi-1	Host (A)	192.168.4.51
tanzu-esxi-3	Host (A)	192.168.4.53
tanzu-vcsa-1	Host (A)	192.168.4.50
tanzu-haproxy-1	Host (A)	192.168.4.2
```

**Prepare a core power shell window and run script to create nested environment**

```
Download PowerShell core 7 msi: https://github.com/PowerShell/PowerShell/releases/tag/v7.0.0-preview.6
Open Power Shell Prompt and run: Install-Module -Name VMware.PowerCLI -Scope CurrentUser
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false
Set-ExecutionPolicy unrestricted
Set-ExecutionPolicy Bypass
Set-PowerCLIConfiguration -Scope User -ParticipateInCEIP $false
Run Script: .\vsphere_basic_with_HA_Proxy(96GWat1).ps1 (make sure vCenter iso is mounted!)
```

**WCP HAproxy enablement inputs**

```
tanzu-haproxy-1
192.168.4.2:5556
wcp/VMware1!
192.168.6.129-192.168.6.190

ssh root@192.168.4.2
root@tanzu-haproxy-1 [ ~ ]# cat /etc/haproxy/ca.crt
-----BEGIN CERTIFICATE-----
MIIDnTCCAoWgAwIBAgIJAJLoLA+LcOoJMA0GCSqGSIb3DQEBBQUAMGwxCzAJBgNV
BAYTAlVTMRMwEQYDVQQIDApDYWxpZm9ybmlhMRIwEAYDVQQHDAlQYWxvIEFsdG8x
DzANBgNVBAoMBlZNd2FyZTENMAsGA1UECwwEQ0FQVjEUMBIGA1UEAwwLMTkyLjE2
OC40LjIwHhcNMjAxMDE4MTU1NzA5WhcNMzAxMDE2MTU1NzA5WjBsMQswCQYDVQQG
EwJVUzETMBEGA1UECAwKQ2FsaWZvcm5pYTESMBAGA1UEBwwJUGFsbyBBbHRvMQ8w
DQYDVQQKDAZWTXdhcmUxDTALBgNVBAsMBENBUFYxFDASBgNVBAMMCzE5Mi4xNjgu
NC4yMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvBuka+1TR3VUH+bu
9R7o8P7a+ArZxop+xbKRkfKwlPXgQ5ZF+gNxoYEXO6DD22CLvOfCTLrf16mpcJko
HRWDF74a9mwYFbfTGRLZSfQ7e/iwdWXtMq+xzdC+Dl4mmeN3vhtK0Q03jKeIhrJb
3me2mFlNwx9nuproCn8j********G3cRM63S34uo3GJlJBH+8vSlIg+6w/ILECp2
dIGz1uVxr6iJlZpT+dOzti3+yFitnhsuHb4e9S0Rp7pyryN9deNaHEvalDZkrLDn
aAYJ5SjScLxPRw9dvgtB4OH32xyFlSahR6Alf+uwXPUqNePNNtP+7+E615Gv/WtE
rW/+TQIDAQABo0IwQDAPBgNVHRMBAf8EBTADAQH/MA4GA1UdDwEB/wQEAwIBhjAd
BgNVHQ4EFgQUlJQbNYCkvZT4kCrDJLgtLi5UUeEwDQYJKoZIhvcNAQEFBQADggEB
AEtpzXYKW77Z7fSXuvUyamXtN4kWqVddmKypalaxns3JdwI3IkfLqCUzOLBKTyJQ
hdBapu4osbenZ1Ch1ZF80x5ZuAVbD9tOYbbpVLMwKzuW+yIaceUIzd6jGgtn+o48
TpseLo9+Y7HXRq0a/lIa2fZ5lQIEEq8M0dj0L2cWwiTG4agzTcsi66UIVYq9LVXj
P6xg8fu/Q8xkagf2N8DWhF+fVLNSEeYVVUJVIaTDMMcqud8Zev1RB61QasotG2LU
tenYfnZWjvPCQ/XLyiV/tfvV/rkXhaEw/QKmW0rNc6nwJvOcSLZpTj4VwGOHTChD
uqpWh2ewsH/5jILtoRFTq8A=
-----END CERTIFICATE-----


DVPG-Supervisor-Management Network
Start: 192.168.4.60
Mask: 255.255.255.0
GW: 192.168.4.1
DNS: 10.197.96.7   
Dom: lab.local
NTP: 10.128.152.81
-
DNS: 10.118.87.50
Workload-Network
GW: 192.168.6.1
Mask: 255.255.255.0
Range: 192.168.6.11-192.168.6.80

```







