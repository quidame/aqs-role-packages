# packages

Ansible role to install or uninstall debian packages. Packages may also be
configured with debconf before installing them, and alternatives can be set
too right after.

This role is a piece of (*yet another*)
[Ansible Quick Starter](/aqs-common) (**AQS**).

## Requirements

Needs jmespath python package installed on the ansible controller.

## Role Variables

The main action the role is called for. Choices are `setup` (the default) and
`unset`.
```yaml
packages__action: unset
```

A complex object that lists the packages to install and optionally their debconf
answers.
```yaml
packages__list:
  - name:
      - vim
      - screen
  - name: locales
    debconf:
      - question: locales/default_environment_locale
        value: fr_FR.UTF-8
        vtype: select
```
If `debconf.name` is not provided, the package name is used instead. This also
allows one to reconfigure another package, not just the one to install.

Options to pass to the `apt` module when installing or uninstalling packages.
All are unset by default, meaning that the default value of the matching module
parameter is applied.
```yaml
packages__allow_unauthenticated: null
packages__autoclean:             null
packages__autoremove:            null
packages__cache_valid_time:      null
packages__default_release:       null
packages__dpkg_options:          null
packages__force:                 null
packages__force_apt_get:         null
packages__install_recommends:    null
packages__only_upgrade:          null
packages__policy_rc_d:           null
packages__purge:                 null
packages__update_cache:          null
packages__upgrade:               null
```

## Dependencies

None.

## Installation

To make use of this role as a galaxy role, put this in `requirements.yml`:

```yaml
- name: "packages"
  src: "https://github.com/quidame/aqs-role-packages.git"
  scm: "git"
  version: master
```

And then run:

```bash
ansible-galaxy install -r requirements.yml
```

## Example Playbook

Configure packages with debconf if wanted, then install all named packages.
```yaml
- hosts: clients
  roles:
    - role: packages
      packages__update_cache; yes
      packages__state: latest
      packages__list:
        - name:
            - apparmor
            - apparmor-utils
        - name: keyboard-configuration
          debconf:
            - question: keyboard-configuration/modelcode
              value: "pc105"
              vtype: string
            - question: keyboard-configuration/layoutcode
              value: "fr,us"
              vtype: string
            - question: keyboard-configuration/variantcode
              value: "latin9,intl"
              vtype: string
            - question: keyboard-configuration/optionscode
              value: "grp:lctrl_lshift_toggle,terminate:ctrl_alt_bksp,grp_led:scroll"
              vtype: string
        - name:
            - screen
            - vim
            - mc
```

Uninstall packages:
```yaml
- hosts: clients
  roles:
    - role: packages
      packages__action: unset
      packages__purge: yes
      packages__list:
        - name: etckeeper
```

## License

GPLv3

## Author Information

<quidame@poivron.org>
