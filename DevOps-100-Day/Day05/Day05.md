# Install and Configuration Selinux

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:

- Install the required SELinux packages.
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Steps

1. Install selinux packages:

    ```sh
    sudo dnf install selinux-policy selinux-policy-targeted policycoreutils policycoreutils-python-utils
    ```

2. Modify file in `/etc/selinux/config:

    ```sh
    sudo vi /etc/selinux/config
    ```

    add this line:

    ```nano
    SELINUX=disabled
    ```

## Good to Know?

### SELinux (Security-Enhanced Linux)

- **Purpose**: Mandatory Access Control (MAC) security framework
- **Modes**: Enforcing, Permissive, Disabled
- **Policies**: Targeted (default), Strict, MLS (Multi-Level Security)
- **Context**: Every file/process has security context (user:role:type:level)

### SELinux States

- **Enforcing**: Policies actively enforced, violations blocked
- **Permissive**: Policies logged but not enforced (audit mode)
- **Disabled**: SELinux completely turned off

### Key Commands

- `getenforce`: Check current SELinux mode
- `setenforce 0/1`: Temporarily set permissive/enforcing
- `sestatus`: Detailed SELinux status
- `sealert`: Analyze SELinux denials

### Configuration Files

- `/etc/selinux/config`: Main configuration
- `/var/log/audit/audit.log`: SELinux violations
- `/etc/selinux/targeted/`: Policy files
---

When we run Linux servers, security is always a big deal. Normal file permissions (read, write, execute) are useful, but not enough in today’s world where apps and services connect to each other in many ways. That’s where SELinux (Security-Enhanced Linux) comes in.

## What is SELinux?
SELinux is a tool built into the Linux kernel that controls what apps, users, and processes can do. It doesn’t just trust normal Linux permissions. Instead, it follows strict rules, called policies, that say exactly what’s allowed and what’s not.
Think of it like a security guard. Even if you have the key to a room (Linux file permissions), the guard (SELinux) might stop you if the rules don’t allow you inside.

## Why use SELinux?
* Extra protection beyond normal file permissions.
* If an app gets hacked, SELinux can stop it from touching sensitive files.
* It logs attempts when something tries to break the rules.
* Keeps the system safer by limiting what apps can do.

## SELinux Modes
SELinux works in three different modes:

* Enforcing – Rules are active, and SELinux blocks anything not allowed.
* Permissive – Rules are checked but not enforced. Violations are only logged. Good for testing.
* Disabled – SELinux is turned off.
Check the current mode with:
```bash
getenforce
```
Or for more details:
```bash
sestatus
```
Installing SELinux
On RHEL, CentOS, Fedora or similar systems:
```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils
```
On Debian/Ubuntu:
```bash
sudo apt update
sudo apt install selinux-basics selinux-policy-default auditd
```
Activate SELinux on Debian/Ubuntu:
```bash
sudo selinux-activate
```
Changing SELinux Settings
The main file to change settings is:
```bash
/etc/selinux/config
```
You’ll see something like this:
```bash
SELINUX=enforcing   # Options: enforcing | permissive | disabled
* SELINUXTYPE=targeted
```
* enforcing = fully active.
* permissive = only logs violations.
* disabled = off.
Change SELinux without reboot
```bash
sudo setenforce 0   # Switch to permissive
sudo setenforce 1   # Switch back to enforcing
```
Change SELinux permanently
Edit the /etc/selinux/config file and set the mode. This takes effect after a reboot.

Useful SELinux Commands
Check denied actions
```bash
sudo cat /var/log/audit/audit.log | grep denied
```
Manage SELinux rules (booleans)
Booleans are switches you can turn on or off to allow certain actions. For example, to let Apache connect to a database:
```bash
sudo setsebool -P httpd_can_network_connect_db on
```
List all available booleans:
```bash
getsebool -a
```
Turning SELinux Off (Not a Good Idea)
If you must disable SELinux:

Open the config file:
```bash
   sudo vi /etc/selinux/config
```
Change this line:
```bash
   SELINUX=disabled
```
Reboot the server.
But remember: turning SELinux off means losing a big layer of protection.

Conclusion
At first, SELinux can feel confusing, but it’s actually a powerful tool for keeping your system safe. A good way to start is to run it in permissive mode so you can see what it would block, then slowly move to enforcing mode when you’re ready.

With SELinux in place, you make it much harder for attackers or buggy apps to harm your system.
