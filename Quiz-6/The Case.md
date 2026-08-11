# Quiz 6 — Ansible Loop

## Instructions

### 1. Create Quiz Directory

Create a new folder in the user home directory. Name it:

```text
quiz-6
```

### 2. Create `group_vars` Directory

Inside the `quiz-6` directory, create a new folder named:

```text
group_vars
```

### 3. Create Variable Files

Inside the `group_vars` folder, create **2 new files** with the following requirements.

#### 3.1 Managed 1

Create the first file named:

```text
managed1-host-<username>.yml
```

This file must contain:

* A list of users from `ops1` until `ops50`.
* A list of users from `dev1` until `dev50`.

Variable names:

```yaml
list_user_ops_managed1
list_user_dev_managed1
```

Example:

```yaml
list_user_ops_managed1:
  - ops1
  - ops2
  # ...
  - ops50

list_user_dev_managed1:
  - dev1
  - dev2
  # ...
  - dev50
```

#### 3.2 Managed 2

Create the second file named:

```text
managed2-host-<username>.yml
```

This file must contain:

* A list of users from `ops51` until `ops100`.
* A list of users from `dev51` until `dev100`.

Variable names:

```yaml
list_user_ops_managed2
list_user_dev_managed2
```

Example:

```yaml
list_user_ops_managed2:
  - ops51
  - ops52
  # ...
  - ops100

list_user_dev_managed2:
  - dev51
  - dev52
  # ...
  - dev100
```

### 4. Create Inventory

Inside the `quiz-6` directory, create an inventory file.

The **section/group name in the inventory must be the same as the file name in the `group_vars` directory**.

For example, if the files are:

```text
group_vars/
├── managed1-host-<username>.yml
└── managed2-host-<username>.yml
```

Then the inventory should contain corresponding groups:

```ini
[managed1-host-<username>]
...

[managed2-host-<username>]
...
```

### 5. Create and Encrypt `secret.yml`

Inside the `quiz-6` directory, create a file named:

```text
secret.yml
```

Store the following variable:

```yaml
pass: adinusa88
```

The file must be encrypted using **Ansible Vault**.

Use:

```text
belajaransible
```

as the Vault encryption password.

Store the Vault password in a file named:

```text
vault-pass
```

Change the permission of `vault-pass` to:

```bash
chmod 600 vault-pass
```

### 6. Create the Ansible Playbook

Inside the `quiz-6` directory, create a playbook named:

```text
quiz-6_loop.yml
```

The playbook must perform the following tasks:

* Create users `dev1`–`dev50` and `ops1`–`ops50` on `pod-<username>-managed1`.
* Create users `dev51`–`dev100` and `ops51`–`ops100` on `pod-<username>-managed2`.
* Each user must have the password:

```text
adinusa88
```

* Use Ansible loops to create the users.
* The password must be obtained from the encrypted `secret.yml` file.
