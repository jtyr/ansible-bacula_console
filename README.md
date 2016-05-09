bacula_console
==============

Ansible role which helps to intall and configure Bacula Console.

The configuraton of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```
---

# Example of how to use the role with default parameters
- hosts: myhost1
  roles:
    - bacula_console

# Example of how to specify the Director address
- hosts: myhost2
  vars:
    bacula_console_config_director_options_address: 192.168.1.123
  roles:
    - bacula_console
```

This role requires [Config
Encoders](https://github.com/jtyr/ansible/blob/jtyr-config_encoders/lib/ansible/plugins/filter/config_encoders.py).
Download the file and put it into the `filter_plugins` directory in the root of
your playbook:

```
$ mkdir ./filter_plugins
$ cd ./filter_plugins
$ curl -O https://github.com/jtyr/ansible/blob/jtyr-config_encoders/lib/ansible/plugins/filter/config_encoders.py
```


Role variables
--------------

List of variables used by the role is as follows:

```
# Package to be installed (exact version can be specified here)
bacula_console_pkg: bacula-console

# Path the to the Drector config file
bacula_console_config_file_path: /etc/bacula/bconsole.conf


# Default values of Director resource options
bacula_console_config_director_options_name: bacula-dir
bacula_console_config_director_options_address: localhost.localdomain
bacula_console_config_director_options_port: 9101
bacula_console_config_director_options_password: bacula

# Default Director resource options
bacula_console_config_director_options__default:
  - Name = {{ bacula_console_config_director_options_name }}
  - Address = {{ bacula_console_config_director_options_address }}
  - DIR Port = {{ bacula_console_config_director_options_port }}
  - Password = {{ bacula_console_config_director_options_password }}

# Custom Director resource options
bacula_console_config_director_options__custom: []

# Final Director resource
bacula_console_config_director:
  - Director: "{{
      bacula_console_config_director_options__default +
      bacula_console_config_director_options__custom
    }}"


# Custom resources (e.g. Console, ConsoleFont)
bacula_console_config_custom: []
# Example of the Console resource
#bacula_console_config_custom:
#  - ConsoleFont:
#    - Name = Default
#    - Font = Monospace 10
#  - Console:
#    - Name = restricted-user
#    - Password = UntrustedUser
```


Dependencies
------------

- [Config Encoders](https://github.com/jtyr/ansible/blob/jtyr-config_encoders/lib/ansible/plugins/filter/config_encoders.py)
- [`bacula_client`](https://github.com/jtyr/ansible-bacula_client) (optional)
- [`bacula_director`](https://github.com/jtyr/ansible-bacula_director) (optional)
- [`bacula_storage`](https://github.com/jtyr/ansible-bacula_storage) (optional)


License
-------

MIT


Author
------

Jiri Tyr
