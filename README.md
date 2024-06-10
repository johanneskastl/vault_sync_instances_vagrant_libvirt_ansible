# vault_sync_instances_vagrant_libvirt_ansible

This Vagrant setup creates two VMs and configures [a Hashicorp Vault
server](https://www.hashicorp.com/products/vault) on both VMs together with
[vault-sync](https://github.com/pbchekin/vault-sync) that synchronizes secrets
from the first to the second Vault instance.

Default OS is openSUSE Leap 15.6. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

The server is only running in development mode. This means that the server does
not need to be unsealed after the start. This also means that all configuration
is only stored in-memory aka is lost after a restart or a reboot of the VM.
Enough for playing around, but **do NOT use this in production**

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Ansible creates a secret on the **first** Vault instance. Log in on the first
   Vault server (`vagrant ssh vault1`) and run the following command:

   ```
   $ vault kv get -mount=secret foo
   = Secret Path =
   secret/data/foo

   ======= Metadata =======
   Key                Value
   ---                -----
   created_time       2024-06-10T05:00:43.18721671Z
   custom_metadata    <nil>
   deletion_time      n/a
   destroyed          false
   version            1

   ==== Data ====
   Key     Value
   ---     -----
   test    vagrant-libvirt
   ```

1. Log in on the second Vault instance and run the same command, and you should
   see the same secret:

   ```
   $ vault kv get -mount=secret foo
   = Secret Path =
   secret/data/foo

   ======= Metadata =======
   Key                Value
   ---                -----
   created_time       2024-06-10T05:00:43.18721671Z
   custom_metadata    <nil>
   deletion_time      n/a
   destroyed          false
   version            1

   ==== Data ====
   Key     Value
   ---     -----
   test    vagrant-libvirt
   ```

1. Creating a secret on the second Vault instance will **not** sync it back to
   the first one.
1. Deleting the secret on the first Vault instance will **not** delete it from
   the second instance.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the files containing the vault tokens, located at
`ansible/vault_root_token--*`.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
