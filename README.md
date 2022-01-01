# ansible_website_hosting_geturl


cat amazon.yml

```
---
- name: "Instaling Apache, PHP"
  yum:
    name:
      - httpd
      - php
    state: present

- name: "Restarting/Enabling Apache service"
  service:
    name: httpd
    state: restarted
    enabled: true

- name: "Downloading website file"
  get_url:
    dest: /tmp/
    url: https://www.tooplate.com/zip-templates/2127_little_fashion.zip
- name: "Extracting files"
  unarchive:
    remote_src: yes
    src: /tmp/2127_little_fashion.zip
    dest: /tmp/
- name: "Copying files to docroot"
  copy:
    remote_src: yes
    src: /tmp/2127_little_fashion/
    dest: /var/www/html/
    owner: apache
    group: apache

```

ubuntu.yml

```
- name: "Instaling Apache, PHP"
  apt:
    name:
      - apache2
      - php
      - unzip
    update_cache: yes
    state: present

- name: "Restarting/Enabling Apache service"
  service:
    name: apache2
    state: restarted
    enabled: true

- name: "Downloading website file"
  get_url:
    dest: /tmp/
    url: https://www.tooplate.com/zip-templates/2127_little_fashion.zip
- name: "Extracting files"
  unarchive:
    remote_src: yes
    src: /tmp/2127_little_fashion.zip
    dest: /tmp/
- name: "Copying files to docroot"
  copy:
    remote_src: yes
    src: /tmp/2127_little_fashion/
    dest: /var/www/html/
    owner: www-data
    group: www-data

```
main.yml

```
---
- name: "Hosting PHP application in Redhat and Debian"
  hosts: all
  become: true
  tasks:
    - include_tasks: amazon.yml
      when: ansible_os_family == "RedHat"
    - include_tasks: ubuntu.yml
      when: ansible_os_family == "Debian"

```
