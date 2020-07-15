# myapp

This role is to  set up the new version of the existing labs website  on a single virtual machine with Ubuntu.


# Prerequisites
We can run this `myapp` role to set-up the new version of existing labs all the prerequisites required for our project is
* nginx
* supervisor
* postgresql
* postgresql-contrib
* git
* python3-pip
* python3-venv
* python-psycopg2
we can install prerequisites using tags by running the following command after apt update
```bash
ansible-playbook -i hosts main.yml --tags=installpkgs
```
## Role variables
The variables used for this role are declared in the `vars` directory for certain values that might need to be changed or edited as per our requirements .  It can be changed in the `vars/all.yml` variable file.
```yml
- name: Adding the systemuser itvadmin with a primary group of itversity
  user:
    name: "{{ user_name }}"
    shell: /bin/bash
    system: yes
    createhome: yes
    home: "{{ user_homedir }}"
    group: "{{ group_name }}"
  tags: createuser
```
## Dependencies
This role contains individual playbooks for each and every tasks, All the playbooks called to run by using the `main.yml` playbook present in the `myapp` role under the `tasks` directory. The `main.yml` playbook is as shown below.
```yml
---
# tasks file for myapp

- import_tasks: packages.yml

- import_tasks: useradd.yml

- import_tasks: createdb.yml

- import_tasks: gitadd.yml

- import_tasks: venv.yml

- import_tasks: updateenv.yml

- import_tasks: gunicorn.yml

- import_tasks: supergunicorn.yml

- import_tasks: nginx.yml
```
# Tags
This is the list of tags present in this role.

| Tags  |  Location |Action   |
|---|---|---|
|aptupdate   |tasks/packages.yml   |Updating apt packages   |
|installpkgs   |tasks/packages.yml  |Installing required pckgs   |
|creategroup   |tasks/useradd.yml   |Creating a systemgroup   |
|createuser   |tasks/useradd.yml   |Creating a systemuser   |
|db   |tasks/createdb.yml   |Creating db for the applicaton   |
|addrepo   |tasks/gitadd.yml   |Adding git repo   |
|gitpull   |tasks/gitadd.yml   |Pulls the code from bitbucket   |
|createvenv   |tasks/venv.yml   |Creates python virtual environment   |
|piprequirements   |tasks/venv.yml   |Installing requirements.txt file in python venv   |
|djangomigrations   |tasks/venv.yml   |Performing django migrations & collect static   |
|envupdate   |tasks/updateenv.yml   |Updating .env file   |
|addlogs   |tasks/gunicorn.yml   |Adding error and out logs for gunicorn   |
|gunicornconf   |tasks/supergunicorn.yml   |Copying gunicorn config file to remote server   |
|restart_supervisor   |tasks/supergunicorn.yml   |Restarting the supervisor service   |
|djanoconf   |tasks/nginx.yml   |Copying django config file to remote server   |
|reloadnginx   |tasks/nginx.yml   |Restarting the nginx service   |

## Variables

This is a list of variables used in this role.

|Variable  |Value   |
|---|---|
|group_name   |itversity   |
|user_name   |itvadmin   |
|user_homedir   |/itvadmin   |
|dbpassword   |5f4dcc3b5aa765d61d8327deb882cf99   |
|dbname   |myapp   |
|postgres_user   |postgres   |
|git_branch   |master   |
|git_user   |saiteja452   |
|repo_owner   |itversity-team   |
|repo_name   |labs-ansible-template   |
|apphost   |"{{ hostvars[groups['app'][0]]['inventory_hostname'] }}"   |

