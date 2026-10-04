Transparent Hugepage Setup
=========

Configures the kernel's transparent hugepage (THP) settings through a `systemd-tmpfiles` configuration file.

The role deploys `/etc/tmpfiles.d/transparent_hugepage_setup.conf`. At every boot, `systemd-tmpfiles-setup.service` writes the configured values into `/sys/kernel/mm/transparent_hugepage/`. That service runs before `sysinit.target`, so the values are in place before any regular service starts and no unit has to declare a dependency on this role.

[![Build Status](https://github.com/Rheinwerk/ansible-role-transparent_hugepage_setup/actions/workflows/ci.yml/badge.svg)](https://github.com/Rheinwerk/ansible-role-transparent_hugepage_setup/actions/workflows/ci.yml)

Requirements
------------

systemd. The settings take effect on the next boot, which makes the role a fit for image builds (Packer and the like). On a running system, apply them right away with:

    systemd-tmpfiles --create /etc/tmpfiles.d/transparent_hugepage_setup.conf

Role Variables
--------------

There is one main variable that drives this role: `_transparent_hugepage_setup`. It is a map that contains all configuration and settings for this role.

`settings` maps a file below `/sys/kernel/mm/transparent_hugepage/` to the value written into it. The defaults are the settings [MongoDB 8.0 recommends](https://www.mongodb.com/docs/manual/tutorial/transparent-huge-pages/) for its tcmalloc-google allocator:

    _transparent_hugepage_setup:
      settings:
        enabled: "always"
        defrag: "defer+madvise"
        khugepaged/max_ptes_none: "0"

To disable THP instead, as recommended for MongoDB 7.0 and earlier:

    _transparent_hugepage_setup:
      settings:
        enabled: "never"
        defrag: "never"

Please see `defaults/main.yml` for details.

Dependencies
------------

None.

Example Playbook
----------------

With the defaults:

    - hosts: servers
      roles:
         - { role: transparent_hugepage_setup, tags: [ 'transparent_hugepage_setup' ] }

With your own settings, following the general contract of taking the variables map from `defaults/main.yml` as a template and passing it as a parameter:

    - hosts: servers
      vars:
        transparent_hugepage_setup:
          settings:
            enabled: "never"
            defrag: "never"
      roles:
         - { role: transparent_hugepage_setup, tags: [ 'transparent_hugepage_setup' ], _transparent_hugepage_setup: "{{ transparent_hugepage_setup }}" }

Upgrading from 0.x
------------------

Versions up to v0.4.0 deployed a oneshot unit `transparent-hugepage-setup.service` without an `[Install]` section, which consumers had to pull in with `Requires=`, and they defaulted to `enabled=never` and `defrag=never`.

Since v1.0.0 the unit is gone and the defaults enable THP. When upgrading:

- remove `Requires=transparent-hugepage-setup.service` and `After=transparent-hugepage-setup.service` from your units
- pass `settings` explicitly if you still need THP disabled

The role does not remove the old unit from hosts that already have it; it targets freshly built images.

License
-------

Please see LICENSE.

Author Information
------------------

Original author is [Daniel Schneller](https://github.com/dschneller) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.
