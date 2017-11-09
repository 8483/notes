# Ansible

What should the server look like vs How to make it that way.  

We configure the state of a server, rather than the installation process with scripts, or god forbid manual installation by typing each command.  

More importantly, it offers built-in monitoring and alerting after deployment, instead of configuring our own for each machine. Ex. Which servers are running which version?  

The configuration scripts serve as documentation compared to reading cryptic bash scripts.  

Ansible is a **declarative** system. Instead of saying **Install something**, we say **I want something to be installed** i.e. if it's already isntalled, do nothing... Otherwise install it.  

Ansible commands are idempotent, meaning they always result in the same, no matter how many times they run.  

Ansible does not require anything to run on the remote servers, aside from SSH access with private/public RSA keys.  

Ansible uses the `jinja2` templating engine to inject variables with `{{variable}}`.  

## Things needed

#### Inventory

A list of machines to do things on. Can be a simple text file.  

```ansible
server1.company.com
server2.company.com
```

Machines can be grouped by using `[group]`. Machines can belong to several groups.  

```ansible
[database]
db1.company.com
db2.company.com
```
## Commands

`ansible database -i invetory -m ping` - Run the ping module (-m) on all the machines in the database group, located in the inventory (-i) list.  

`ansible database -i inventory -m apt -a "name=mysql-server state=present"` - If mysql is not installed, install it on all machines missing it in the database group.  

## Playbook  

Instead of running commands one by one, we can define a playbook(.yml) with all the commands in one place.  

Each task should have a name, so that it's printed in the output as it runs, as well as serve as documentation.  

```yml
--
- hosts: all
    tasks:
        - name: Update package list
            apt: update_cache=yes cache_valid_time=36000 # last 10 hours.

- hosts: database
    tasks:
        - name: Install MySQL
            apt: name=mysql-server state=present
        - name: Copy fixtures
            template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
```

`ansible-playbook -i inventory playbook.yml` - Run the playbook.  

#### Variables

To prevent repetition, we can define variables and inject them where needed.  

```yml
--
- hosts: database
    vars:
        fixture_name: fixtures.sql # Variable
    tasks:
        - name: Install MySQL
            apt: name=mysql-server state=present
        - name: Copy fixtures
            copy: src={{fixture_name}} dest=/tmp/{{fixture_name}}
```

#### Loops

```yml
--
- hosts: all
    tasks:
        - name: Install packages
            apt: name={{item}} state=present
            with_items: # Loop
                - python
                - python-pip
                - vim
```

#### Loops with Variables

```yml
--
- hosts: all
    vars:
        packages: # Variable
            - python
            - python-pip
            - vim
    tasks:        
        - name: Install packages
            apt: name={{item}} state=present
            with_items: packages # Variable
```

## File Organization Convention
