# Automation for WireGuard VPN tunnels

## Setting up VPN servers

Provision any number of machines based on Debian 10 and create/update `hosts.cfg`, e.g.:

```ini
[vpn]
vpn-01 ansible_host=42.0.0.1
```

The default WireGuard listening port is `10000`. It is possible to customize the port per machine:

```ini
[vpn]
vpn-01 ansible_host=42.0.0.1 wireguard_port=12345
```

After that, run:

```shell
ansible-playbook playbook.yml
```

## Setting up VPN clients

Set up any number of WireGuard clients with unique IPs and create/update `wireguard.yml`, e.g.:

```yml
---
wireguard_peers:
  client-01:
    host: 10.0.0.2
    pubkey: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
```

In some cases (e.g. on Android), it is convenient to import WireGuard configurations with the private key included. It is possible to also specify the private key, which will be injected into the generated tunnel configuration file:

```yml
---
wireguard_peers:
  client-01:
    host: 10.0.0.2
    privkey: QQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQQ=
    pubkey: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
```

After that, run:

```shell
ansible-playbook playbook.yml --tags wireguard
```

Once the playbook finishes, the VPN tunnel configurations for every client will be exported under `configs/`, e.g. `configs/client-01/wg-vpn-01.conf` for the infrastructure described above.

## License

[0-clause BSD](LICENSE-0BSD.txt)
