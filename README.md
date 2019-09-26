# packages

Ansible role to install or uninstall debian packages.

This role is a piece of (*yet another*)
[Ansible Quick Starter](/aqs-common) (**AQS**).

## Requirements

None.

## Role Variables

The main action the role is called for. Choices are `setup` (the default) and
`unset`.
```yaml
packages__action: unset
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

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

Install packages:
```yaml
- hosts: all
  roles:
    - role: packages
      packages__name: ["vim", "screen", "mc"]
      packages__update_cache; yes
      packages__state: latest
```

Uninstall packages:
```yaml
- hosts: servers
  roles:
    - role: packages
      packages__action: unset
      packages__name: etckeeper
      packages__purge: yes
```

## License

GPLv3

## Author Information

<quidame@poivron.org>
