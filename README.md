cyberarkpassword_plugin
=======================

cyberarkpassword lookup plugin to retrieve credentials from Cyberark digital Vault using AIM.

For Ansible on windows, please change the -parameters (-p, -d, and -o) to /parameters (/p, /d, and /o) and change the location of CLIPasswordSDK.exe

**Note**: To use the plugin if not part of core ansible, please edit your ansible.cfg to include in lookup_plugins the following path /etc/ansible/roles/enunez-cyberark.cyberark_password_lookup_plugin/lookup_plugins


Requirements
------------

- CyberArk Application Identitity Manager (AIM) Credential Provider in ansible server.
- /opt/CARKaim/sdk/clipasswordsdk in place

plugin arguments
----------------

**Note**: Parameter names AppID, Query, Output are case sensitive.
- **AppID** (str): Defines the unique ID of the application that is issuing the password request.
- **Query** (str): Describes the filter criteria for the password retrieval.
- **Output** (str): Specifies the desired output fields separated by commas. They could be: Password, PassProps.<property>, PasswordChangeInProcess

Optionally, you can specify extra parameters recognized by clipasswordsdk (like FailRequestOnPasswordChange, Queryformat, Reason, etc.)

plugin return
-------------

- **dict**: A dictionary with 'password' as key for the credential, passprops.<property>, passwordchangeinprocess

If the specified property does not exist for this password, the value <na> will be returned for this property.

If the value of the specified property is empty, <null> will be returned.


For extraParms values please check parameters for clipasswordsdk in CyberArk's "Credential Provider and ASCP Implementation Guide"


Example Playbook
----------------

Example playbook showing how to retrieve credentials from CyberArk Digital Vault using cyberarkpassword lookup plugin. 

```
---
- hosts: localhost
  vars:
   - password_object: "{{lookup('cyberarkpassword', AppID='app_ansible', Query='safe=CyberArk_Passwords;folder=root;object=AdminPass', Output='Password,PassProps.UserName,PassProps.Address,PasswordChangeInProcess')}}"

  tasks:
   - debug: msg="the value of password is {{password_object.password}}  userName={{password_object.passprops.username}} address={{password_object.passprops.address}} passwordchangeinprocess={{password_object.passwordchangeinprocess}} inventory_hostname={{inventory_hostname}} ansible_hostname={{ansible_hostname}}"
```

License
-------

MIT

Author Information
------------------

- Edward Nunez (edward.nunez@cyberark.com)
