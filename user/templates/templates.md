---
lang: en
layout: doc
permalink: /doc/templates/
redirect_from:
- /doc/template/
- /en/doc/templates/
- /doc/Templates/
- /wiki/Templates/
ref: 131
title: Templates
---


In [How to Get Started](/doc/how-to-get-started/), we covered the distinction in Qubes OS between where you *install* your software and where you *run* your software.
Your software is installed in **templates**.
Each template shares its root filesystem (i.e., all of its programs and system files) with all the qubes based on it.
**App qubes** are where you run your software and store your data.

The template system has significant benefits:

* **Security:** Each qube has read-only access to the template on which it's based, so if a qube is compromised, it cannot infect its template or any of the other qubes based on that template.
* **Storage:** Each qube based on a template uses only the disk space required to store its own data (i.e., your files in its home directory), which dramatically saves on disk space.
* **Speed:** It is extremely fast to create new app qubes, since the root filesystem already exists in the template.
* **Updates:** Updates are naturally centralized, since updating a template means that all qubes based on it will automatically use those updates after they're restarted.

An important side effect of this system is that any software installed in an app qube (rather than in the template on which it is based) will disappear after the app qube reboots (see [Inheritance and Persistence](#inheritance-and-persistence)).
For this reason, we recommend installing most of your software in templates, not app qubes.

The default template in Qubes is based on Fedora, but there are additional templates based on other Linux distributions.
There are also templates available with or without certain software preinstalled.
You may find it useful to have multiple templates installed in order to provide:

* Different security levels (e.g., more or less trusted software installed)
* Different environments (e.g., Fedora, Debian, Whonix)
* Different tools (e.g., office, media, development, hardware drivers)

## Official

These are the official Qubes OS Project templates.
We build and release updates for these templates.
We guarantee that the binary updates are compiled from exactly the same source code as we publish.

* [Fedora](/doc/templates/fedora/) (default)
* [Fedora Minimal](/doc/templates/minimal/)
* [Fedora Xfce](/doc/templates/xfce)
* [Debian](/doc/templates/debian/)
* [Debian Minimal](/doc/templates/minimal/)

## Community

These templates are supported by the Qubes community.
Some of them are available in ready-to-use binary package form (built by the Qubes developers), while others are available only in source code form.
In all cases, the Qubes OS Project does not provide updates for these templates.
However, such updates may be provided by the template maintainer.

By installing these templates, you are trusting not only the Qubes developers and the distribution maintainers, but also the template maintainer.
In addition, these templates may be somewhat less stable, since the Qubes developers do not test them.

* [Whonix](/doc/templates/whonix/)
* [Ubuntu](/doc/templates/ubuntu/)
* [Arch Linux](/doc/building-archlinux-template/)
* [CentOS](/doc/templates/centos/)
* [CentOS Minimal](/doc/templates/minimal/)
* [Gentoo](/doc/templates/gentoo/)
* [Gentoo Minimal](/doc/templates/minimal/)

## Installing

Certain templates come preinstalled with Qubes OS.
However, there may be times when you wish to install a fresh template from the Qubes repositories, e.g.:

* When a template version you're using reaches [end-of-life](/doc/supported-versions/).
* When a new version of a template that you wish to use becomes [supported](/doc/supported-versions/).
* When you suspect your template has been compromised.
* When you have made modifications to your template that you no longer want.

Please refer to each template's installation instructions.
Usually, the installation method is to execute the following type of command in dom0:

```
$ sudo qubes-dom0-update qubes-template-<name>
```

where `qubes-template-<name>` is the name of your template package.

If you wish to install a community template, you must enable the community template repo:

```
$ sudo qubes-dom0-update --enablerepo=qubes-templates-community qubes-template-<name>
```

If you receive the message that no match is found for `qubes-template-<name>`, see [here](/faq/#when-i-try-to-install-a-template-it-says-no-match-is-found).

## After Installing

After installing a fresh template, we recommend performing the following steps:

1. [Update the template](#updating).

2. [Switch any app qubes that are based on the old template to the new one](#switching).

3. If desired, [uninstall the old template](#uninstalling).

## Updating

Please see [How to Update](/doc/how-to-update/).

## Installing Software

Please see [How to Install Software](/doc/how-to-install-software).

## Uninstalling

The procedure for uninstalling a template depends on how it was created.

If the template was originaly created by cloning another template, then you can delete it the same way as you would any other qube.
In the Qube Manager, right-click on the template and select **Delete qube**.
(If you're not sure, you can safely try this method first to see if it works.)

If, on the other hand, the template came pre-installed or was installed by installing a template package in dom0, per the instructions [above](#installing), then you must execute the following type of command in dom0 in order to uninstall it:

    $ sudo dnf remove qubes-template-<name>

(where `qubes-template-<name>` is the name of your template package)

You may see warning messages like the following:

```
warning: file /var/lib/qubes/vm-templates/fedora-XX/whitelisted-appmenus.list: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/vm-whitelisted-appmenus.list: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/root.img.part.04: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/root.img.part.03: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/root.img.part.02: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/root.img.part.01: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/root.img.part.00: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/netvm-whitelisted-appmenus.list: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/icon.png: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/clean-volatile.img.tar: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/apps.templates: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/apps.tempicons: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX/apps: remove failed: No such file or directory
warning: file /var/lib/qubes/vm-templates/fedora-XX: remove failed: No such file or directory
```

These are normal and expected. Nothing is wrong, and no action is required to address these warnings.

If this uninstallation command doesn't work, please see [How to Remove VMs Manually](/doc/remove-vm-manually/).

If the Applications Menu entry doesn't go away after you uninstall a template, execute the following type of command in dom0:

```
$ rm ~/.local/share/applications/<template_vm_name>
```

Applications Menu entries for backups of removed VMs can also be found in `/usr/local/share/applications/` of dom0.

```
$ rm /usr/local/share/applications/<template_vm_name>
```

## Reinstalling

Please see [How to Reinstall a Template](/doc/reinstall-template/).

## Switching

When you install a new template or upgrade a clone of a template, it is recommended that you switch everything that was set to the old template to the new template:

1. Make the new template the default template.

    `
    Applications Menu --> System Tools --> Qubes Global Settings --> Default template
    `

2. If your keyboard or mouse is connected through `sys-usb`, switch `sys-usb` to the new template.
   (Note that this is a single command to ensure that `sys-usb` restarts.
   If it does not, you will not be able to use your USB keyboard or mouse.)

    ```
    [user@dom0 ~]$ qvm-shutdown --wait sys-usb; qvm-prefs sys-usb template <new_template>; qvm-start sys-usb
    ```

3. Base app qubes on the new template.

    ```
    Applications Menu --> System Tools --> Qubes Template Manager
    ```

4. Base the dispoable qube template on the new template.

    ```
    [user@dom0 ~]$ qvm-create -l red -t <new_template> <new_template_dvm>
    [user@dom0 ~]$ qvm-prefs <new_template_dvm> template_for_dispvms True
    [user@dom0 ~]$ qvm-features <new_template_dvm> appmenus-dispvm 1
    [user@dom0 ~]$ qubes-prefs default-dispvm <new_template_dvm>
    ```

## Advanced

The following sections cover advanced topics pertaining to templates.

### Inheritance and Persistence

Whenever an app qube is created, the contents of the `/home` directory of its parent template are *not* copied to the child app qube's `/home`.
The child app qube's `/home` is always independent from its parent template's `/home`, which means that any subsequent changes to the parent template's `/home` will not affect the child app qube's `/home`.

Once an app qube has been created, any changes in its `/home`, `/usr/local`, or `/rw/config` directories will be persistent across reboots, which means that any files stored there will still be available after restarting the app qube.
No changes in any other directories in app qubes persist in this manner. If you would like to make changes in other directories which *do* persist in this manner, you must make those changes in the parent template.

|                | Inheritance (1)                                           | Persistence (2)
|----------------|-----------------------------------------------------------|------------------------------------------
|Template        | n/a                                                       | Everything
|App qube (3)    | `/etc/skel` to `/home`, `/usr/local.orig` to `/usr/local` | `/rw` (includes `/home`, `/usr/local` and `bind-dirs`)
|Disposable qube | `/rw` (includes `/home`, `/usr/local` and `bind-dirs`)    | Nothing

(1) Upon creation
(2) Following shutdown
(3) Including any disposable qube templates

### Trusting your templates

As the template is used for creating filesystems for other app qubes where you actually do the work, it means that the template is as trusted as the most trusted app qube based on this template.
In other words, if your template gets compromised, e.g. because you installed an application, whose *installer's scripts* were malicious, then *all* your app qubes (based on this template) will inherit this compromise.

There are several ways to deal with this problem:

* Only install packages from trusted sources -- e.g. from the pre-configured Fedora repositories.
  All those packages are signed by Fedora, and we expect that at least the package's installation scripts are not malicious.
  This is enforced by default (at the [firewall VM level](/doc/firewall/)), by not allowing any networking connectivity in the default template, except for access to the Fedora repos.

* Use *standalone VMs* (see below) for installation of untrusted software packages.

* Use multiple templates (see below) for different classes of domains, e.g. a less trusted template, used for creation of less trusted app qubes, would get various packages from less trusted vendors, while the template used for more trusted app qubes will only get packages from the standard Fedora repos.

Some popular questions:

> So, why should we actually trust Fedora repos -- it also contains large amount of third-party software that might be buggy, right?

As far as the template's compromise is concerned, it doesn't really matter whether `/usr/bin/firefox` is buggy and can be exploited, or not.
What matters is whether its *installation* scripts (such as %post in the rpm.spec) are benign or not.
A template should be used only for installation of packages, and nothing more, so it should never get a chance to actually run `/usr/bin/firefox` and get infected from it, in case it was compromised.
Also, some of your more trusted app qubes would have networking restrictions enforced by the [firewall VM](/doc/firewall/), and again they should not fear this proverbial `/usr/bin/firefox` being potentially buggy and easy to compromise.

> But why trust Fedora?

Because we chose to use Fedora as a vendor for the Qubes OS foundation (e.g. for Dom0 packages and for app qube packages).
We also chose to trust several other vendors, such as Xen.org, kernel.org, and a few others whose software we use in Dom0.
We had to trust *somebody* as we are unable to write all the software from scratch ourselves.
But there is a big difference in trusting all Fedora packages to be non-malicious (in terms of installation scripts) vs. trusting all those packages are non-buggy and non-exploitable.
We certainly do not assume the latter.

> So, are the templates as trusted as Dom0?

Not quite.
Dom0 compromise is absolutely fatal, and it leads to Game Over<sup>TM</sup>.
However, a compromise of a template affects only a subset of all your app qubes (in case you use more than one template, or also some standalone VMs).
Also, if your app qubes are network disconnected, even though their filesystems might get compromised due to the corresponding template compromise, it still would be difficult for the attacker to actually leak out the data stolen in an app qube.
Not impossible (due to existence of cover channels between VMs on x86 architecture), but difficult and slow.

### Note on treating app qubes' root filesystem non-persistence as a security feature

Any app qube that is based on a template has its root filesystem non-persistent across VM reboots.
In other words, whatever changes the VM makes (or the malware running in this VM makes) to its root filesystem, are automatically discarded whenever one restarts the VM.

This might seem like an excellent anti-malware mechanism to be used inside the VM.
However, one should be careful with treating this property as a reliable way to keep the VM malware-free.
This is because the non-persistence, in the case of normal VMs, applies only to the root filesystem and not to the user filesystem (on which the `/home`, `/rw`, and `/usr/local` are stored) for obvious reasons.
It is possible that malware, especially malware that could be specifically written to target a Qubes-based VMs, could install its hooks inside the user home directory files only.
Examples of obvious places for such hooks could be: `.bashrc`, the Firefox profile directory which contains the extensions, or some PDF or DOC documents that are expected to be opened by the user frequently (assuming the malware found an exploitable bug in the PDF or DOC reader), and surely many others places, all in the user's home directory.

One advantage of the non-persistent rootfs though, is that the malware is still inactive before the user's filesystem gets mounted and "processed" by system/applications, which might theoretically allow for some scanning programs (or a skilled user) to reliably scan for signs of infections of the app qube.
But, of course, the problem of finding malware hooks in general is hard, so this would work likely only for some special cases (e.g. an app qube which doesn't use Firefox, as otherwise it would be hard to scan the Firefox profile directory reliably to find malware hooks there).
Also note that the user filesystem's metadata might got maliciously modified by malware in order to exploit a hypothetical bug in the app qube kernel whenever it mounts the malformed filesystem.
However, these exploits will automatically stop working (and so the infection might be cleared automatically) after the hypothetical bug got patched and the update applied (via template update), which is an exceptional feature of Qubes OS.

Also note that disposable qubes do not have persistent user filesystem, and so they start up completely "clean" every time.
Note the word "clean" means in this context: the same as their template filesystem, of course.

### Important Notes

* `qvm-trim-template` is no longer necessary or available in Qubes 4.0 and higher.
  All VMs are created in a thin pool and trimming is handled automatically.
  No user action is required.
  See [Disk Trim](/doc/disk-trim) for more information.

* RPM-installed templates are "system managed" and therefore cannot be backed up using Qubes' built-in backup function.
  In order to ensure the preservation of your custom settings and the availability of a "known-good" backup template, you may wish to clone the default system template and use your clone as the default template for your app qubes.

* Some templates are available in ready-to-use binary form, but some of them are available only as source code, which can be built using the [Qubes Builder](/doc/qubes-builder/).
  In particular, some template "flavors" are available in source code form only.
  For the technical details of the template system, please see [Template Implementation](/doc/template-implementation/).
  Take a look at the [Qubes Builder](/doc/qubes-builder/) documentation for instructions on how to compile them.

