# Security



### SELinux

Config:

**/etc/selinux/config**

**/etc/sysconfig/selinux**&#x20;

**tools**

* **setenforce**
* **getenforce**
* semanage
* restorecon
* **chcon**

### Seccomp

\> The idea behind seccomp is to restrict the system calls that can be made from a process

Some secomp profiles can be found here:  [https://kubernetes.io/docs/tutorials/clusters/seccomp/#create-seccomp-profiles](https://kubernetes.io/docs/tutorials/clusters/seccomp/#create-seccomp-profiles)

Profiles are referenced at POD `spec.securityContext.seccompProfile`-&#x20;

``

References:

* [https://lwn.net/Articles/656307/](https://lwn.net/Articles/656307/)
* [https://kubernetes.io/docs/tutorials/clusters/seccomp/](https://kubernetes.io/docs/tutorials/clusters/seccomp/)







*
* [trivy](https://github.com/aquasecurity/trivy)
* [falco](https://falco.org)&#x20;
