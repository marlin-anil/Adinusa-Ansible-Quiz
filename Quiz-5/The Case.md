# Quiz 5 — Ansible Secrets

## Instructions

### 1. Create Working Directory

Create a new directory named:

```text
quiz-5
```

This directory will be used as the working directory for the quiz.

---

### 2. Create Ansible Configuration and Inventory

Inside the `quiz-5` directory, create:

```text
ansible.cfg
inventory
```

Define the following managed hosts:

```text
pod-<username>-managed1
pod-<username>-managed2
```

Both hosts must belong to a group named:

```ini
[managed]
```

Example:

```ini
[managed]
pod-<username>-managed1
pod-<username>-managed2
```

> Replace `<username>` with the username provided by the lab environment.

---

### 3. Create Encrypted Secret File

Inside the `quiz-5` working directory, create an encrypted file named:

```text
quiz-Secret.yml
```

The Vault encryption password must be:

```text
adinusa
```

The encrypted file must contain the following variables:

```yaml
username: <username>
passwd: adinusa2023
```

For example:

```yaml
username: lina
passwd: adinusa2023
```

After creating the file, encrypt it using **Ansible Vault**.

---

### 4. Create the Playbook

Create a playbook named:

```text
quiz-5_secrets.yml
```

The playbook must use the variables defined in the encrypted:

```text
quiz-Secret.yml
```

The playbook should retrieve:

```text
username
passwd
```

from the encrypted file.

---

### 5. Create the User

Configure the playbook to create a user based on the `username` variable.

The user's password must be set using the value stored in the `passwd` variable.

Conceptually:

```text
quiz-Secret.yml
       │
       ├── username
       │       ↓
       │   Create user
       │
       └── passwd
               ↓
         Set user password
```

---

### 6. Create Vault Password File

Create a password file named:

```text
quiz-pass
```

The file must contain the plain-text Vault password:

```text
adinusa
```

Change the file permission so that it can only be accessed by its owner.

Example:

```bash
chmod 600 quiz-pass
```

Verify the permission:

```bash
ls -l quiz-pass
```

Expected permission:

```text
-rw------- 
```

---

### 7. Run the Playbook

Run the playbook using the Vault password file:

```bash
ansible-playbook -i inventory quiz-5_secrets.yml --vault-password-file quiz-pass
```

The playbook should successfully:

1. Read the encrypted `quiz-Secret.yml`.
2. Decrypt it using the password stored in `quiz-pass`.
3. Retrieve the `username` variable.
4. Create the specified user.
5. Retrieve the `passwd` variable.
6. Set the user's password to `adinusa2023`.
