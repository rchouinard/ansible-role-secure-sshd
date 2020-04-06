# Secure SSHd Ansible Role

This role provides a modern secure shell server configuration based on
[Mozilla's Infrasec guidelines](https://infosec.mozilla.org/guidelines/openssh).

The default configuration does allow password-based authentication. If you prefer
to only allow key-based authentication, be sure to adjust `sshd_authmethods`
accordingly.

## Requirements

* Ansible 2.4+

## Role Variables

``` yaml
sshd_hostkeys:
  - /etc/ssh/ssh_host_ed25519_key
  - /etc/ssh/ssh_host_rsa_key
  - /etc/ssh/ssh_host_ecdsa_key

sshd_kexalgorithms:
  - curve25519-sha256@libssh.org
  - ecdh-sha2-nistp521
  - ecdh-sha2-nistp384
  - ecdh-sha2-nistp256
  - diffie-hellman-group-exchange-sha256

sshd_ciphers:
  - chacha20-poly1305@openssh.com
  - aes256-gcm@openssh.com
  - aes128-gcm@openssh.com
  - aes256-ctr
  - aes192-ctr
  - aes128-ctr

sshd_macs:
  - hmac-sha2-512-etm@openssh.com
  - hmac-sha2-256-etm@openssh.com
  - umac-128-etm@openssh.com
  - hmac-sha2-512
  - hmac-sha2-256
  - umac-128@openssh.com

sshd_authmethods:
  - publickey
  - password

sshd_loglevel: VERBOSE

sshd_subsystems:
  - "sftp  /usr/libexec/openssh/sftp-server -f AUTHPRIV -l INFO"

sshd_permitrootlogin: "no"

sshd_extra: |
  AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
  AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
  AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
  AcceptEnv XMODIFIERS
```

## Dependencies

None.

## Example Playbook

``` yaml
- hosts: localhost
  roles:
    - rchouinard.secure_sshd
```

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
