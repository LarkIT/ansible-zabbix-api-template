# Lark IT Ansible Zabbix API Template Role

Manipualtes Zabbix templates via the Zabbix API

## Zabbix Versions
- Zabbix 4.2

## Dependencies
- This role has no dependencies on other roles

## Variables
| Variable | Required? | Default Value | Type | Description |
|----------|--------|-------|------|--------|
| zabbix_server_url | Yes | N/A | String | The URL where the Zabbix web service is listening |
| zabbix_login_user | Yes | N/A | String | The username Ansible uses to authenticate to Zabbix |
| zabbix_login_password | Yes | N/A | String | The password Ansible users to authenitcate to Zabbix |
| zabbix_template_name | Yes | N/A | String | The name of the Zabbix template to create or dump |
| zabbix_template_state | No | present | string | The desired state of the Zabbix template. Set to present to create, and dump to dump the template to a file |
| zabbix_template_template | Yes, when state is `create` | N/A | String | The name of the template file to reder and import into Zabbix to create the template |
| zabbix_template_trigger_dependencies | No | [] | List | A list of Zabbix trigger expressions that trigger(s) in this template should depend on. See the template files for specifics. | 

## Notes
- `ansible_connection` should be set to `local` for this role. 
- When zabbix_template_state set to `dump`, the resulting json file will be dumped in the playbook's working directory

## Example create

```yaml
- name: Create Zabbix Template 
  include_role: 
    name: zabbix-api-template
  vars: 
      zabbix_server_url: https://zabbix.example.com
      zabbix_login_user: user
      zabbix_login_password: pass
      zabbix_template_name: Template Module ICMP Ping Branch GW
      zabbix_template_template: zabbix-template-icmp.json.j2
      zabbix_template_trigger_dependencies: [{"name": "{HOST.NAME} unavailable by ICMP ping",
                                              "expression": "{main-gw:icmpping.max(#3)}=0",
                                              "recovery_expression": ""}]
```

## Example dump

```yaml
- role: zabbix-api-template
  vars: 
    zabbix_server_url: https://zabbix.example.com
    zabbix_login_user: user
    zabbix_login_password: pass
    zabbix_template_state: dump
    zabbix_template_name: Template
```