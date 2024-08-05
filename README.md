# ansible-role-tinc

## What it is

Install and configure simple Tinc VPN networks

## How to use

To use this role simply define, at a minimum, the `tinc_networks` variable and execute the role agaisn't your host(s).

One thing to keep in mind is how this role manages the RSA key-pair:
  1. If a private key does not exist AND is not defined in `tinc_networks` it will be generated.
  2. If a public key is not defined it will be generated based on the private key and printed. In this case you must add it your `tinc_networks` definition and re-run the role.
  3. If you do store the private key in Ansible, be sure to store it in an Ansible Vault.

## Role Variables

| Variable         | Default   | Description                                     |
|------------------|-----------|-------------------------------------------------|
| `tinc_user`      | root      | The user to permission as                       |
| `tinc_group`     | root      | The group to permission as                      |
| `tinc_directory` | /etc/tinc | The base tinc configuration directory           |
| `tinc_networks`  | []        | A list of dictionaries of networks to configure |

## Default Playbook
```
---
- hosts: node
  become: true
  gather_facts: true
  roles:
    - ansible-role-tinc
  vars:
    # ansible-role-tinc
    tinc_networks:
      - network_name: "cyberpunk"
        host_name: "lucy"
        interface: "tun0"
        ip: "172.24.16.2"
        mask: "26"
        connect_to: "delamain"
        rsa_public_key: |
          -----BEGIN RSA PUBLIC KEY-----
          MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqskqZbuN6CfD1XQIMHfZ
          e0SX/5S8VRAG0KPDUwAGcOGy4nn9rhEvKJvtddjjgwHkrM9Ze2u0SEsfmOTc5gBg
          W+wqZBU335IlY/EBV5/vWOtXSDmPUsGyP0Fc07TxqwdVv3YziLwVit4+HEZmaEXI
          794DPbaGICcYMMR0qJoYwFytoTpLZn13AWoYO88VXJHfMsH5NQ6QoCxa4dCuRamy
          s3c4WCtWpgrtRT/EmeT+CCG9veWX12ToTNotEa6GsuVS7yqeGW40DMPpunEfwleG
          WdqIN88M208x3cCQHvbLpXRqDlqr8XkEHb144lYHALldTKDbDLkeHkTBZlHP4U1/
          uQIDAQAB
          -----END RSA PUBLIC KEY-----
        hosts:
          - host_name: "delamain"
            address: "35.212.81.255"
            port: "655"
            rsa_public_key: |
                -----BEGIN RSA PUBLIC KEY-----
                MIIBCgKCAQEA6u1UJxH7H2qnQzbsdbHCD6tkVxeYC0B5bXu2T2zz+oGtKsBGe33f
                DhDg1KiX/WsrCZqKXQRBAOPDCmxxatNbKAv8h8uLAWUh5ibrxcwuE6+KhGmkmTkE
                dAnur8n4Mr+rM0YOBqzGE4oIfmjn/9CWZBh1gknw7WI9mPGvNe0368qEfviabqcR
                a9EOzqRK91QhmC+ZZQmm1bx8OsmpT5m5b1FgxMprYk/pTVi5wa8FyGg7ELjkYwzM
                nvuSNAyHkHfbz957mlaqQc/0Y/DHA+4Od9jlE85u4ev++UYhWTq/EiwmZ0Ehx9K+
                EOCLzLbeS6SiUqVN7YOy85qN2IkKLYezGwIDAQAB
                -----END RSA PUBLIC KEY-----
```
