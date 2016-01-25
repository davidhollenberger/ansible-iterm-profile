iTerm Dynamic Profile
=========

Ansible role to create iTerm dynamic profiles using Ansible's AWS dynamic inventory.  It will generate one json file per AWS dynamic inventory.

An overview of iTerm Dynamic Profiles can be found [here](https://iterm2.com/dynamic-profiles.html).

For more information on setting up the Ansible's AWS dynamic inventory see documentation [here](http://docs.ansible.com/ansible/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script).

Requirements
------------

* iTerm2 2.9.20140923 or later ([download here](https://iterm2.com/downloads.html))


Role Variables
--------------

**extra-vars**

* `hosts` Ansible host group to gather facts to base dynamic inventory on.  You need to have ssh access to all hosts in the group for this role to work.

**defaults/main.yml**

* `name_vars` List of host level variables that will be used to create the iTerm profile name.  The first item is required.  The rest are optional.
* `tag_vars` List of variables to create iTerm tags from.
* `env_tag` name of environment that is being generated.  Override value by passing in extra-vars or from inventory.
* `profile_filename` name of json file that is generated.  Default is `aws_{{ env_tag }}`.
* `parent_profile` The dynamic profile will inherit properties from this existing iTerm profile.


Example Playbook
----------------

```yaml
# Usage: ansible-playbook iterm_dynamic_profile.yml -i <path_to_inventory> --extra-vars="hosts=key_AnsibleKeyPair"

- name: Gather Facts on AWS Instances
  hosts: {{ host_groups }}
  connection: ssh
  tasks:
    - name: Gather EC2 facts
      ec2_facts:

- name: Generate iTerm Profile
  hosts: localhost
  connection: local
  roles:
    - role: ansible-iterm-profile
```


License
-------

BSD

Author Information
------------------

[David Hollenberger](davidhollenberger.com)
