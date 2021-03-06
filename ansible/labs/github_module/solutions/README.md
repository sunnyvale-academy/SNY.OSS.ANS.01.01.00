Create an unencrypted variable file:


```yaml
---
github_token: my-super-secret-token
```

Encrypt the variable file using Ansible Vault:

```console
$ ansible-vault create vault
New Vault password: 
Confirm New Vault password:
```

Ensure the file has been encrypted:

```console
$ cat vault
$ANSIBLE_VAULT;1.1;AES256
66623031636566653439636132336463343266636537333565646332643734383039656237643130
6166373661623138663039336335336561656137663863360a326538356331653432333432306436
65653862356665383539313734353735323165343232653538663638386565313338636431363130
3262393631356637390a656630383531316666383432343531353430626236366633373561643166
66356233323231356661666666326232353633373333343939326330383666656264663436333137
38643830396136653364636539643366393961353632643636393332306664303264373833656333
663735633039316165386561363039353566
```

Reference the encrypted variable file in the playbook

```yaml
- hosts: localhost
  vars_files:
    - vault
...
```

Reference the ecnypted variable in the playbook (using **vault_** prefix)

```yaml
  vars: 
    - github_token: "{{ vault_github_token }}"
...
```

Run the command

```console
$ ansible-playbook \
    --ask-vault-pass \
    -vvv \
    -e username=myuser \
    -e repo_name=myrepo \
    github_repo_playbook.yml
Vault password: 
...
```