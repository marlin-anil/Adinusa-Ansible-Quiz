Create a new directory named quiz-4 for project directory.

Create inventory, define pod-username-managed1 and pod-username-managed2 in a group named managed.

Create a new directory named roles on quiz-4 directory.

Create the directory structure for a role called quiz-roles.

Edit the roles/quiz-roles/tasks/main.yml. The role should perform the following tasks:

Task that install the apache2 package.

Task that ensure the apache2 service is started and enabled.

Task that use the template module to ensure quiz-roles.conf.j2 is installed to /etc/apache2/sites-available/quiz-roles.conf on managed hosts.

Task that enable webserver configuration using a2ensite command. in addition, add notify module that notify restart apache2 service handler.

Task that using copy module to perform copy from html-quiz/ to /var/www/quiz-roles/{{ ansible_hostname }} on managed host.

Edit the roles/quiz-roles/handlers/main.yml file. The content is handler that restart the apache2 service.

Create index.html file that contains "adinusa lab quiz roles - username" on roles/quiz-roles/files/html-quiz/ directory.

Create the quiz-roles.conf.j2 file for webserver configuration template in the roles/quiz-roles/templates/ directory. Use quiz-roles.username-adinusa as the ServerName and make sure the DocumentRoot and Directory are pointed to the index.html location on the managed host.

Write a playbook that uses the role, named quiz-4_roles.yml. After that, save and run the playbook.

Verification
Make sure the ansible.cfg, inventory, quiz-4_roles.yml files and roles directory exist on quiz-002-1 directory.

apache2 package are installed on managed host.

apache2 service is running on managed host.

Make sure the /var/www/quiz-roles/pod-username-managed1/index.html file is exist in the pod-username managed1. as well as in pod-username-managed2.

Test webserver on pod-username-managed1 and pod-username-managed2. The output should 'adinusa lab quiz roles - username' .
