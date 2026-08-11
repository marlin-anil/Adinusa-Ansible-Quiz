# Quiz 4 - Ansible Roles

## 1. Create Project Directory

Create a new directory named `quiz-4` for the project.

```bash
mkdir quiz-4
cd quiz-4
```

## 2. Create Inventory

Create an `inventory` file and define the managed hosts in the `managed` group.

```bash
vi inventory
```

```ini
[managed]
pod-username-managed1
pod-username-managed2
```

## 3. Create Roles Directory

Create the `roles` directory inside `quiz-4`.

```bash
mkdir roles
```

## 4. Create Role Directory Structure

Create the directory structure for the `quiz-roles` role.

```bash
mkdir -p roles/quiz-roles/tasks
mkdir -p roles/quiz-roles/handlers
mkdir -p roles/quiz-roles/files/html-quiz
mkdir -p roles/quiz-roles/templates
```

The directory structure should be:

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

## 5. Configure Role Tasks

Edit `roles/quiz-roles/tasks/main.yml`.

```bash
vi roles/quiz-roles/tasks/main.yml
```

```yaml
---
- name: Install apache2 package
  ansible.builtin.apt:
    name: apache2
    state: present
    update_cache: yes

- name: Ensure apache2 service is started and enabled
  ansible.builtin.service:
    name: apache2
    state: started
    enabled: yes

- name: Install quiz-roles webserver configuration
  ansible.builtin.template:
    src: quiz-roles.conf.j2
    dest: /etc/apache2/sites-available/quiz-roles.conf
  notify: Restart apache2

- name: Enable webserver configuration
  ansible.builtin.command:
    cmd: a2ensite quiz-roles.conf
  notify: Restart apache2

- name: Copy html-quiz files
  ansible.builtin.copy:
    src: html-quiz/
    dest: "/var/www/quiz-roles/{{ ansible_hostname }}/"
    mode: "0644"
```

## 6. Configure Apache Handler

Edit `roles/quiz-roles/handlers/main.yml`.

```bash
vi roles/quiz-roles/handlers/main.yml
```

```yaml
---
- name: Restart apache2
  ansible.builtin.service:
    name: apache2
    state: restarted
```

## 7. Create index.html

Create the `index.html` file inside `roles/quiz-roles/files/html-quiz/`.

```bash
vi roles/quiz-roles/files/html-quiz/index.html
```

Content:

```text
adinusa lab quiz roles - username
```

## 8. Create Apache Webserver Template

Create `quiz-roles.conf.j2` inside the `templates` directory.

```bash
vi roles/quiz-roles/templates/quiz-roles.conf.j2
```

Content:

```apache
<VirtualHost *:80>
    ServerName quiz-roles.username-adinusa

    DocumentRoot /var/www/quiz-roles/{{ ansible_hostname }}

    <Directory /var/www/quiz-roles/{{ ansible_hostname }}>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/quiz-roles-error.log
    CustomLog ${APACHE_LOG_DIR}/quiz-roles-access.log combined
</VirtualHost>
```

The `{{ ansible_hostname }}` variable will automatically be replaced with the hostname of each managed host.

For example:

```text
pod-username-managed1
```

will become:

```text
/var/www/quiz-roles/pod-username-managed1
```

And:

```text
pod-username-managed2
```

will become:

```text
/var/www/quiz-roles/pod-username-managed2
```

## 9. Create the Playbook

Create `quiz-4_roles.yml`.

```bash
vi quiz-4_roles.yml
```

Content:

```yaml
---
- name: Configure Apache using quiz-roles
  hosts: managed
  become: true

  roles:
    - quiz-roles
```

## 10. Verify Project Structure

Run:

```bash
tree
```

Expected output:

```text
quiz-4/
├── inventory
├── quiz-4_roles.yml
└── roles
    └── quiz-roles
        ├── files
        │   └── html-quiz
        │       └── index.html
        ├── handlers
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        └── templates
            └── quiz-roles.conf.j2
```

## 11. Test Ansible Inventory

Check the inventory:

```bash
ansible-inventory -i inventory --graph
```

Expected:

```text
@all:
  |--@managed:
  |  |--pod-username-managed1
  |  |--pod-username-managed2
```

## 12. Test Connectivity

Test connectivity to the managed hosts:

```bash
ansible managed -i inventory -m ping
```

Expected:

```text
pod-username-managed1 | SUCCESS => {
    "ping": "pong"
}

pod-username-managed2 | SUCCESS => {
    "ping": "pong"
}
```

## 13. Run the Playbook

Run the playbook:

```bash
ansible-playbook -i inventory quiz-4_roles.yml
```

Expected result:

```text
PLAY RECAP
pod-username-managed1    : ok=...
pod-username-managed2    : ok=...
```

## 14. Verification

### Verify Required Files

Make sure the following files and directories exist on `quiz-002-1`:

```text
ansible.cfg
inventory
quiz-4_roles.yml
roles/
```

Check with:

```bash
ls -la
```

Check the role:

```bash
find roles -type f
```

### Verify Apache2 Package

Check that Apache2 is installed:

```bash
ansible managed -i inventory -b -m shell -a "dpkg -l apache2 | grep '^ii'"
```

### Verify Apache2 Service

Check that Apache2 is running:

```bash
ansible managed -i inventory -b -m command -a "systemctl is-active apache2"
```

Expected:

```text
active
```

Check that Apache2 is enabled:

```bash
ansible managed -i inventory -b -m command -a "systemctl is-enabled apache2"
```

Expected:

```text
enabled
```

### Verify index.html on Managed1

Check the `index.html` file on `pod-username-managed1`:

```bash
ansible pod-username-managed1 -i inventory -b -m command \
  -a "cat /var/www/quiz-roles/pod-username-managed1/index.html"
```

Expected:

```text
adinusa lab quiz roles - username
```

### Verify index.html on Managed2

Check the `index.html` file on `pod-username-managed2`:

```bash
ansible pod-username-managed2 -i inventory -b -m command \
  -a "cat /var/www/quiz-roles/pod-username-managed2/index.html"
```

Expected:

```text
adinusa lab quiz roles - username
```

### Verify Apache Configuration

Check that the Apache configuration exists:

```bash
ansible managed -i inventory -b -m command \
  -a "ls -l /etc/apache2/sites-available/quiz-roles.conf"
```

Check that the site is enabled:

```bash
ansible managed -i inventory -b -m command \
  -a "ls -l /etc/apache2/sites-enabled/quiz-roles.conf"
```

## 15. Test Webserver

Test the webserver on `pod-username-managed1`:

```bash
ansible pod-username-managed1 -i inventory -b -m command \
  -a "curl http://localhost"
```

Expected:

```text
adinusa lab quiz roles - username
```

Test the webserver on `pod-username-managed2`:

```bash
ansible pod-username-managed2 -i inventory -b -m command \
  -a "curl http://localhost"
```

Expected:

```text
adinusa lab quiz roles - username
```

You can also test using the configured `ServerName`:

```bash
curl -H "Host: quiz-roles.username-adinusa" http://localhost
```

Expected output:

```text
adinusa lab quiz roles - username
```

## 16. Final Checklist

- [ ] Create `quiz-4` directory
- [ ] Create `inventory`
- [ ] Define `pod-username-managed1` in `managed` group
- [ ] Define `pod-username-managed2` in `managed` group
- [ ] Create `roles` directory
- [ ] Create `quiz-roles` role structure
- [ ] Configure `roles/quiz-roles/tasks/main.yml`
- [ ] Install Apache2
- [ ] Start and enable Apache2
- [ ] Create `quiz-roles.conf.j2`
- [ ] Enable Apache configuration using `a2ensite`
- [ ] Create Apache2 restart handler
- [ ] Create `index.html`
- [ ] Copy HTML files to managed hosts
- [ ] Create `quiz-4_roles.yml`
- [ ] Run the playbook successfully
- [ ] Verify Apache2 package
- [ ] Verify Apache2 service
- [ ] Verify `index.html` on managed1
- [ ] Verify `index.html` on managed2
- [ ] Test webserver on managed1
- [ ] Test webserver on managed2
- [ ] Verify output: `adinusa lab quiz roles - username`
