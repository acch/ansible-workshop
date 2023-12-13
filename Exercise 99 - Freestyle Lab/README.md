![Ansible logo](../img/ansible.png)

# Exercise 99 — Freestyle Lab

![Goals](../img/goals.png)

## Objective

In this exercise, students will explore external resources for integrating IBM Storage products with Red Hat Ansible. The external documentation (blog articles, README, etc.) is the primary guidance for this exercise.

Alternatively, students can develop their own, custom automation scenarios — using the provided lab infrastructure as sandbox environment.

![Paper clip](../img/paper--clip.png)

## Reference Information

The following sections provide links to resources for Ansible integration of select IBM Storage products. Use this as guidance for developing your own automation scenarios...

### Linux System Roles

- [Linux System Roles](https://linux-system-roles.github.io/)
- [GitHub repo](https://github.com/linux-system-roles)
- [Ansible Galaxy](https://galaxy.ansible.com/ui/repo/published/fedora/linux_system_roles/)
- [Red Hat Enterprise Linux (RHEL) System Roles](https://access.redhat.com/articles/3050101) (includes example usage section)

Installation:

```shell
dnf install rhel-system-roles
```

### Spectrum Virtualize

- [`ibm.spectrum_virtualize`](https://galaxy.ansible.com/ui/repo/published/ibm/spectrum_virtualize/)
- [Example 1](https://github.ibm.com/Systems-LabServices-Ansible/Spectrum_Virtualize)
- [Example 2](https://github.com/upenr/ansible-ibm-spectrum-virtualize)
- [Managing your Spectrum Virtualize with Ansible](https://medium.com/possimpible/managing-your-spectrum-virtualize-with-ansible-part-1-6f3ec173948f)

Installation:

```shell
ansible-galaxy collection install ibm.spectrum_virtualize
```

### Spectrum Scale

- [`acch.spectrum_scale`](https://galaxy.ansible.com/ui/standalone/roles/acch/spectrum_scale/)
- [GitHub repo](https://github.com/acch/ansible-scale) (Community Roles)
- [GitHub repo](https://github.com/IBM/ibm-spectrum-scale-install-infra) (Official Roles)
- [Official Roles Example](https://github.ibm.com/Systems-LabServices-Ansible/spectrum-scale)

Installation:

```shell
ansible-galaxy install acch.spectrum_scale
```

A Spectrum Scale installation repository is available to you as:

- URL: <http://infraserv/yum/gpfs>
- Version: 5.0.5.3

### Spectrum Control

- [Automated IBM Spectrum Control installation with Ansible](https://medium.com/possimpible/automated-ibm-spectrum-control-installation-with-ansible-2593d9e93691)
- [`olemyk.ansible_spectrum_control`](https://galaxy.ansible.com/ui/standalone/roles/olemyk/ansible_spectrum_control/)
- [`olemyk.ansible_role_db2`](https://galaxy.ansible.com/ui/standalone/roles/olemyk/ansible_role_db2/)

Installation:

```shell
ansible-galaxy install olemyk.ansible_spectrum_control
```
