iTerm Dynamic Profile
=========

Ansible role to create iTerm dynamic profiles using Ansible's AWS dynamic inventory.  It will generate one json file per AWS dynamic inventory.  By default they will be created in `~/Library/Application Support/iTerm2/DynamicProfiles/` and iTerm will scan then update the profiles when the json files are created or updated.

If iTerm has problems scanning the dynamic inventory files, it will log errors in `/var/log/system.log`.

An overview of iTerm Dynamic Profiles can be found [here](https://iterm2.com/dynamic-profiles.html).

For more information on setting up the Ansible's AWS dynamic inventory see documentation [here](http://docs.ansible.com/ansible/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script).

Requirements
------------

* iTerm2 2.9.20140923 or later ([download here](https://iterm2.com/downloads.html))


Role Variables
--------------

**extra-vars**

* `hosts` Ansible host group to base dynamic inventory on.  You need to have ssh access to all hosts in the group for this role to work.

**defaults/main.yml**

* `name_vars` List of host level variables that will be used to create the iTerm profile name.  The first item is required.  The rest are optional.
* `tag_vars` List of variables to create iTerm tags from.
* `env_tag` name of environment that is being generated.  Override value by passing in extra-vars or from inventory.
* `profile_filename` name of json file that is generated.  Default is `aws_{{ env_tag }}`.
* `parent_profile` Dynamic profiles can inherit existing iTerm profiles.  This role will use the naming standard `aws_{{ env_tag }}` (ie, aws_production).  If those profiles do not exist, the dynamic profile will inherit the *Default* profile.  



Example Playbook
----------------

Follow these steps to setup and run this role using an existing AWS dynamic inventory.

* Create `requirements.yml` file:

  ```yaml
  ---
  # This is used to manage Ansible role dependences
  # http://docs.ansible.com/galaxy.html#installing-multiple-roles-from-a-file
  # Install with: `ansible-galaxy install -r requirements.yml`

  - name: ansible-iterm-profile
    src: https://github.com/davidhollenberger/ansible-iterm-profile
    path: roles/
  ```

* Create `iterm_dynamic_profile.yml` file:

  ```yaml
  ---
  # Usage: ansible-playbook iterm_dynamic_profile.yml -i <path_to_inventory> --extra-vars="hosts=key_AnsibleKeyPair"

  - name: Gather Facts on AWS Instances
    hosts: "{{ hosts }}"
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

* Install role with ansible-galaxy:

  ```
  ansible-galaxy install -r requirements.yml
  ```

* Run Playbook:

  ```
  ansible-playbook iterm_dynamic_profile.yml -i <path_to_inventory> --extra-vars="hosts=key_AnsibleKeyPair
  ```


License
-------

BSD

Author Information
------------------

[David Hollenberger](davidhollenberger.com)
