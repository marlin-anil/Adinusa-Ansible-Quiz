## Verification

After running the playbook, perform the following verification steps.

### 1. Verify Project Files

Make sure the following files and directories exist inside the `quiz-002-1` directory:

```text
quiz-002-1/
├── ansible.cfg
├── inventory
├── quiz-4_roles.yml
└── roles/
```

---

### 2. Verify Apache2 Package

Verify that the `apache2` package is installed on both managed hosts.

Example:

```bash
ansible managed -m ansible.builtin.package -a "name=apache2"
```

The package should be reported as installed.

---

### 3. Verify Apache2 Service

Verify that the `apache2` service is running on both managed hosts.

Example:

```bash
ansible managed -m ansible.builtin.service -a "name=apache2"
```

The service should be in a running state.

---

### 4. Verify `index.html`

Make sure the following file exists on **managed1**:

```text
/var/www/quiz-roles/pod-<username>-managed1/index.html
```

Also verify the file exists on **managed2**:

```text
/var/www/quiz-roles/pod-<username>-managed2/index.html
```

The expected directory structure on the managed hosts is:

```text
/var/www/
└── quiz-roles/
    ├── pod-<username>-managed1/
    │   └── index.html
    │
    └── pod-<username>-managed2/
        └── index.html
```

---

### 5. Test the Web Server

Test the web server on:

```text
pod-<username>-managed1
pod-<username>-managed2
```

The web server should return the following output:

```text
adinusa lab quiz roles - <username>
```

For example, if the username is `lina`:

```text
adinusa lab quiz roles - lina
```

Both managed hosts must return the expected output.

### Expected Result

The configuration is considered successful when:

* `ansible.cfg` exists.
* `inventory` exists.
* `quiz-4_roles.yml` exists.
* `roles/` directory exists.
* `apache2` is installed on both managed hosts.
* `apache2` is running on both managed hosts.
* `index.html` exists in the correct `/var/www/quiz-roles/` directory on both managed hosts.
* The web server on both managed hosts returns:

```text
adinusa lab quiz roles - <username>
```
