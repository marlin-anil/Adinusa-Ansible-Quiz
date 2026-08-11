## Verification

After running the playbook, perform the following verification steps.

### 1. Verify Required Files

Make sure the following files exist inside the `quiz-5` directory:

```text
quiz-5/
├── ansible.cfg
├── inventory
├── quiz-5_secrets.yml
├── quiz-Secret.yml
└── quiz-pass
```

---

### 2. Verify Secret and Password Files

Verify that `quiz-Secret.yml` is encrypted using Ansible Vault.

The file should contain encrypted content beginning with something similar to:

```text
$ANSIBLE_VAULT;1.1;AES256
```

The decrypted content must contain:

```yaml
username: <username>
passwd: adinusa2023
```

Verify that `quiz-pass` contains the Vault password:

```text
adinusa
```

Also verify that the password file has the correct permission:

```bash
ls -l quiz-pass
```

The expected permission is:

```text
-rw-------
```

or:

```text
600
```

---

### 3. Verify the User on Managed Hosts

Verify that the user defined by the `username` variable exists on the managed hosts.

For example:

```bash
ansible managed -m ansible.builtin.command -a "id <username>"
```

The command should return information about the created user.

The user's password should be:

```text
adinusa2023
```

You can verify the password by attempting to authenticate as the created user on the managed hosts.

### Expected Result

The verification is successful when:

* `ansible.cfg` exists.
* `inventory` exists.
* `quiz-5_secrets.yml` exists.
* `quiz-Secret.yml` exists and is encrypted.
* `quiz-pass` exists and contains `adinusa`.
* `quiz-pass` has permission `600`.
* The user defined by `username` exists on the managed hosts.
* The user's password is set to `adinusa2023`.
