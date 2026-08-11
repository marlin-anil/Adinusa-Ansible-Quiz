## Verification

1. Make sure these files exist in `quiz-002-3`:

```text
ansible.cfg
inventory
quiz-6_loop.yml
secret.yml
vault-pass
```

2. Make sure all file contents are correct.

3. Make sure each user list has **51 lines**:

   * 1 line for the variable name
   * 50 lines for users

4. This lab is **case-sensitive**. Make sure all filenames, variables, usernames, and hostnames are written correctly.

5. Make sure the users can log in.

Example:

```bash
for i in {1..50}; do sshpass -p adinusa88 ssh dev$i@pod-username-managed1 whoami; done
```

The output should show:

```text
dev1
dev2
dev3
...
dev50
```
