Transparent Hugepage Setup
=========

This role can be used to set up modified transparent_hugepage settings

[![Build Status](https://github.com/Rheinwerk/ansible-role-transparent_hugepage_setup/actions/workflows/ci.yml/badge.svg)](https://github.com/Rheinwerk/ansible-role-transparent_hugepage_setup/actions/workflows/ci.yml)

Requirements
------------

None.

Role Variables
--------------

There is one main variable that drives this role: `_transparent_hugepage_setup`. It is a map that contains all configuration and settings for this role.
Please see `defaults/main.yml` for details.

Dependencies
------------

None.


Example Playbook
----------------

The general contract of this role is to take the variables map `_transparent_hugepage_setup` from `defaults/main.yml` as a template for your configuration and pass that configuration as a parameter to this role.

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      var:
        transparent_hugepage_setup:
          ...
      roles:
         - { role: transparent_hugepage_setup, tags: [ 'transparent_hugepage_setup' ], _transparent_hugepage_setup: "{{ transparent_hugepage_setup }}" }

License
-------

Please see LICENSE.

Author Information
------------------

Original author is [Daniel Schneller](https://github.com/dschneller) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.

