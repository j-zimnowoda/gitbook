# Security



### SELinux

Config:

**/etc/selinux/config**

**/etc/sysconfig/selinux**&#x20;

### sestatus

```
> sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      32
```

### **Default policies**

#### **targeted**

only network service processes are BUT it enforces memory restrictions for all processes

The file structure of targeted policy is the following:

```
/etc/selinux/targeted
├── booleans.subs_dist
├── contexts
│   ├── customizable_types
│   ├── dbus_contexts
│   ├── default_contexts
│   ├── default_type
│   ├── <service name>_context
│   ├── files
│   │   ├── file_contexts
│   ├── users
│   │   ├── <user name>
├── logins
├── setrans.conf
└── seusers
```

#### minimum

tbd

#### mls

tbd

### Context&#x20;

Contexts are labels applied to files, directories, ports, and processes. There are four SELinux contexts: User, Role, Type, and Level. The most common context is `Type` context.

The label naming convention determines that type context labels should end with **\_t**, as in **kernel\_t**.

Some rules:

* created files inherit context of their parent
* moving/coping files preserve context what can cause problems
* it is possible to reset context to inherit from new parent by calling **restorecon** command

### Debugging SELinux&#x20;

Enable trouble shooting utilities

```
sudo yum install setroubleshoot-server
```

After installing this package you will find `setroubleshoot` logs in `/var/log/messages` file. There will be a refernece to command that produces details about alert, e.g.: `sealert -l fe373c17-8e52-4b31-a1be-c336cb88f496`

``

**Other utilities**

* **setenforce**
* **getenforce**
* semanage
* restorecon
* **chcon**

### Seccomp

\> The idea behind seccomp is to restrict the system calls that can be made from a process

* Some secomp profiles can be found here:  [https://kubernetes.io/docs/tutorials/clusters/seccomp/#create-seccomp-profiles](https://kubernetes.io/docs/tutorials/clusters/seccomp/#create-seccomp-profiles)
* It is enabled per node at kubelete.&#x20;
* Profiles are referenced at POD `spec.securityContext.seccompProfile`

#### Check if seccomp is enabled

```
grep CONFIG_SECCOMP= /boot/config-$(uname -r)
CONFIG_SECCOMP=y
```

#### References:

* [https://lwn.net/Articles/656307/](https://lwn.net/Articles/656307/)
* [https://kubernetes.io/docs/tutorials/clusters/seccomp/](https://kubernetes.io/docs/tutorials/clusters/seccomp/)







*
* [trivy](https://github.com/aquasecurity/trivy)
* [falco](https://falco.org)&#x20;
