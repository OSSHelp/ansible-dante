# dante

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/dante/status.svg)](https://drone.osshelp.ru/ansible/dante)

Role for Dante installation and configuration.

## Usage (example)

```yaml
    - role: dante
```

Although, it will use the default configuration, which could not suit the target environment (e.g. protocols and interfaces). Usage example with custom parameters:

```yaml
    - role: dante
      dante_internal:
        - '0.0.0.0 port = 1080'
        - ':: port = 1080'
      dante_internal_protocol:
        - ipv4
        - ipv6
      dante_external:
        - eth0
      dante_external_protocol:
        - ipv4
        - ipv6
      dante_socksmethod: 'pam.username none'
      dante_clientmethod: 'none'
      dante_pam_auth:
        user: 'some_username'
        password: 'password123'
      dante_access_rules:
        - { name: test,
            mode: block,
            from: '192.0.2.22/24',
            to: '0.0.0.0/0',
            log: 'error'
        }
      dante_command_rules:
        - { comment: test,
            action: block,
            socksmethod: none,
            from: '0.0.0.0/0',
            to: 'www.example.org',
            command: 'bind connect udpassociate',
            protocol: tcp udp,
            log: 'error'
        }
```

## Available parameters

### Main

| Param | Default | Description |
| -------- | -------- | -------- |
| `dante_setup` | `full` | Setup mode. See [OSSHelp KB article](https://oss.help/kb4895) |
| `dante_internal` | see [defaults](defaults/main.yml) | Array to describe where daemon should listen on. |
| `dante_internal_protocol` | `[ipv4, ipv6]` | Array, describing which protocols should be used for incoming traffic. |
| `dante_external` | `[eth0]` | Array, describing which interfaces should be used for outgoing traffic. |
| `dante_external_protocol` | `[ipv4, ipv6]` | Array, describing which protocols should be used for outgoing traffic. |
| `dante_socksmethod` | - | Sets socks authentication method, see [the docs](https://www.inet.no/dante/doc/latest/config/auth.html)|
| `dante_clientmethod` | - | Sets client authentication method, see [the docs](https://www.inet.no/dante/doc/1.3.x/config/server.html) |
| `dante_access_rules` | - | Array for setting client access rules, see [the docs](https://www.inet.no/dante/doc/1.3.x/config/server.html) and example above. |
| `dante_command_rules` | - | Array for setting command rules, see [the docs](https://www.inet.no/dante/doc/1.3.x/config/server.html) and example above. |
| `systemd_unit_override` | - | Options for enable systemd unit override for dante (false by default). |
| `dante_pam_auth.user`, `dante_pam_auth.password` | - | Username and password for [PAM Authentication](https://www.inet.no/dante/doc/1.4.x/config/auth_pam.html) |

## FAQ

None, so far.

## Useful links

- [Official documentation](https://www.inet.no/dante/doc/1.3.x/config/server.html)

## TODO

- add focal support

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
