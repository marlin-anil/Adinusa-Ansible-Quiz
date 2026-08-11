# Quiz 4 — Ansible Roles

## Instructions

### 1. Create Project Directory

Create a new directory named:

```text
quiz-4
```

This directory will be used as the project directory.

---

### 2. Create Inventory

Create an inventory file inside the `quiz-4` directory.

Define the following hosts:

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

### 3. Create `roles` Directory

Inside the `quiz-4` directory, create a directory named:

```text
roles
```

The project structure should begin like this:

```text
quiz-4/
├── inventory
└── roles/
```

---

### 4. Create Ansible Role

Create the directory structure for an Ansible role named:

```text
quiz-roles
```

The expected structure is:

```text
quiz-4/
├── inventory
└── roles/
    └── quiz-roles/
        ├── tasks/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── files/
        │   └── html-quiz/
        └── templates/
```

---

### 5. Configure `tasks/main.yml`

Edit:

```text
roles/quiz-roles/tasks/main.yml
```

The role must perform the following tasks.

#### Task 1 — Install Apache2

Install the `apache2` package.

#### Task 2 — Start and Enable Apache2

Ensure that the `apache2` service:

* Is started.
* Is enabled at boot.

#### Task 3 — Deploy Web Server Configuration

Use the **template** module to install:

```text
quiz-roles.conf.j2
```

to:

```text
/etc/apache2/sites-available/quiz-roles.conf
```

on the managed hosts.

#### Task 4 — Enable Web Server Configuration

Use the `a2ensite` command to enable:

```text
quiz-roles.conf
```

The task must also use `notify` to trigger a handler that restarts the `apache2` service.

#### Task 5 — Copy Website Files

Use the **copy** module to copy the contents of:

```text
html-quiz/
```

to:

```text
/var/www/quiz-roles/{{ ansible_hostname }}
```

on the managed hosts.

---

### 6. Configure Apache2 Handler

Edit:

```text
roles/quiz-roles/handlers/main.yml
```

Create a handler that restarts the `apache2` service.

The handler should be triggered by the `notify` directive from the web server configuration task.

---

### 7. Create `index.html`

Create:

```text
roles/quiz-roles/files/html-quiz/index.html
```

The file must contain:

```text
adinusa lab quiz roles - <username>
```

Replace `<username>` with your lab username.

Example:

```html
adinusa lab quiz roles - lina
```

---

### 8. Create Apache Template

Create:

```text
roles/quiz-roles/templates/quiz-roles.conf.j2
```

This file will be used as the Apache web server configuration template.

The configuration must:

* Use the following as `ServerName`:

```text
quiz-roles.<username>-adinusa
```

* Point `DocumentRoot` to the location of `index.html` on the managed host.
* Point the Apache `<Directory>` configuration to the same website directory.

The expected website directory is:

```text
/var/www/quiz-roles/{{ ansible_hostname }}
```

Therefore, both `DocumentRoot` and `<Directory>` should point to this directory.

---

### 9. Create the Playbook

Create a playbook named:

```text
quiz-4_roles.yml
```

The playbook must use the `quiz-roles` role.

The expected project structure is:

```text
quiz-4/
├── inventory
├── quiz-4_roles.yml
└── roles/
    └── quiz-roles/
        ├── tasks/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── files/
        │   └── html-quiz/
        │       └── index.html
        └── templates/
            └── quiz-roles.conf.j2
```

---

### 10. Run the Playbook

Save all files and run the playbook:

```bash
ansible-playbook -i inventory quiz-4_roles.yml
```

After the playbook finishes successfully, verify that Apache2 is running and that the website is accessible on the managed hosts.
