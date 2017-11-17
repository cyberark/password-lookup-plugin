cyberarkpassword_plugin
=======================

cyberarkpassword lookup plugin to retrieve credentials from Cyberark digital Vault using AIM.

For Ansible on Windows, please change the -parameters (`-p`, `-d`, and `-o`) to `/parameters` (`/p`, `/d`, and `/o`) and change the location of `CLIPasswordSDK.exe`

**Note**: To use the plugin if not part of core ansible, please edit your `ansible.cfg` to include in `lookup_plugins` the following path `/etc/ansible/roles/cyberark.cyberark_password_lookup_plugin/lookup_plugins`


Requirements
------------

- CyberArk Application Identitity Manager (AIM) Credential Provider in ansible server.
- CyberArk AIM Installed, and `/opt/CARKaim/sdk/clipasswordsdk` in place or set environment variable `AIM_CLIPASSWORDSDK_CMD` to the AIM
CLI Password SDK executable.

Plugin Usage
------------

```jinja
{{ lookup("cyberarkpassword", {"appid": "app_ansible", "query": "safe=CyberArk_Passwords;folder=root;object=AdminPass",
                               "output": "Password,PassProps.UserName,PassProps.Address,PasswordChangeInProcess"}) }}
```

OR

```yml
with_cyberarkpassword:
  appid: 'app_ansible'
  query: 'safe=CyberArk_Passwords;folder=root;object=AdminPass'
  output: 'Password,PassProps.UserName,PassProps.Address,PasswordChangeInProcess'
```


Plugin Arguments
----------------
- **`appid`** (str): Defines the unique ID of the application that is issuing the password request.
- **`query`** (str): Describes the filter criteria for the password retrieval.
- **`output`** (str): Specifies the desired output fields separated by commas. They could be: `Password`, `PassProps.<property>`, `PasswordChangeInProcess`

Optionally, you can specify extra parameters recognized by clipasswordsdk (like FailRequestOnPasswordChange, Queryformat, Reason, etc.)

 Plugin Return
 -------------
- **`dict`**: A dictionary with '`password`' as key for the `credential`, `passprops.<property>`, `passwordchangeinprocess`

If the specified property does not exist for this password, the value <na> will be returned for this property.

If the value of the specified property is empty, <null> will be returned.


for extra_parms values please check parameters for clipasswordsdk in CyberArk's "Credential Provider and ASCP Implementation Guide"

For Ansible on Windows, please change the -parameters (`-p`, `-d`, and `-o`) to `/parameters` (`/p`, `/d`, and `/o`) and change the location of `CLIPasswordSDK.exe`


Example Playbook
----------------

Example playbook showing how to retrieve credentials from CyberArk Digital Vault using cyberarkpassword lookup plugin.

```yml
---
- hosts: localhost

  tasks:
    - debug:
        msg: '{{ item }}'
      with_cyberarkpassword:
        appid: 'app_ansible'
        query: 'safe=CyberArk_Passwords;folder=root;object=AdminPass'
        output: 'Password,PassProps.UserName,PassProps.Address,PasswordChangeInProcess'

    - debug:
        msg: '{{ lookup("cyberarkpassword", {"appid": "app_ansible", "query": "safe=CyberArk_Passwords;folder=root;object=AdminPass", "output": "Password,PassProps.UserName,PassProps.Address,PasswordChangeInProcess"}) }}'
```

License
-------

MIT

Author Information
------------------

- Edward Nunez (edward.nunez@cyberark.com)
