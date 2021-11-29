# Ansible Server NextCloud Role  
## Summary
__Graph:__
``` mermaid
graph LR;
  subgraph Roles
    common_role-->homeassistant_role;
    common_role-.->caddy_role;
  end
  subgraph Hosts
    homeassistant_role-->HomeAssistant;
    caddy_role-.->Caddy;
    Router-.Ports 80,443.->Caddy;
    Caddy-.Port 80.->HomeAssistant;
  end
```
This roles setups up a [HomeAssistant](https://www.home-assistant.io/) server. This role setup and installs Mariadb, and HomeAssistant. When a Caddy reverse proxy is provided in the ansible inventory file the HomeAssistant hosts firewall will be opened allowing caddy to access HomeAssistant on port 80, create a Caddy proxy file, etc.

## Requirements
### ansible-galaxy
__Collections:__
  - [community.general](https://docs.ansible.com/ansible/latest/collections/community/general/index.html)

__Roles:__
  - [daemonslayer2048.common](https://github.com/Daemonslayer2048/common_role)

## Variables
| Variable | Type | Required | Default | Example |
|    -     |   -  |     -    |    -    |    -    |
| [hostname](#hostname) | string | True | N/A | cloud |
| [host_public_domain](#host_public_domain) | string | True | N/A | null.com |
| [hass_user](#ass_user) | string | True | hass | hass |
| [hass_user_home](#hass_user_home) | string | True | /srv/homeassistant | /srv/homeassistant |
| [mysql_encoding](#mysql_encoding) | string | True | utf8mb4 | utf8mb4 |
| [mysql_application_database](#mysql_application_database) | string | True | homeassistant | homeassistant |
| [mysql_application_user](#mysql_application_user) | string | True | hass | hass |
| [mysql_root_password](#mysql_root_password) | string | True | N/A | Password1! |
| [mysql_application_password](#mysql_application_password) | string | True | N/A | Password1! |


### Variable Summaries:
#### hostname
This is used in multiple locations to set the FQDN of the server and to be used in setting the public URL for the server when the [Caddy reverse proxy role](https://github.com/Daemonslayer2048/caddy_role) is deployed.

#### host_public_domain
Completes the domain portion of the public URL with the help of the variable above.

#### hass_user  
The name of the user HomeAssistant will run as on the host

#### hass_user_home  
The name of the user HomeAssistant will run as on the host

#### mysql_encoding
The mysql database encoding to set, this should most likely not be changed.

#### mysql_application_database
The name of the database NextCloud should use.

#### mysql_application_user
The user Nextcloud will use to authenticate to the database.

#### mysql_root_password
The root password to set for the Mariadb service.

#### mysql_application_password
The application user NextCloud will use for authentication to the server.


## Notes:
### Testing Issues
  - Selinux Modules can not be tested in podman so these tests are performed outside of the stesting suite :(
