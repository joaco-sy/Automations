# deploy to tomcat [FILE - WIP]

### Task: Copy new_war to the /home Directory

- name: copy del new_war /home
  import_tasks: copy-war.yml

This task imports the tasks defined in the copy-war.yml file and executes them. The content of the copy-war.yml file is not provided here, but it is likely to contain tasks for copying the new_war file to the /home directory.

---

### Task: Stop the Tomcat Service

- name: parar el servicio tomcat
  import_tasks: stop-tomcat.yml

This task imports the tasks defined in the stop-tomcat.yml file and executes them. The content of the stop-tomcat.yml file is not provided here, but it is likely to contain tasks for stopping the Tomcat service.

---

### Task: Backup the Old WAR File

- name: copy old_war -> /bkp & delete old
  import_tasks: backup.yml

This task imports the tasks defined in the backup.yml file and executes them. The content of the backup.yml file is not provided here, but it is likely to contain tasks for creating a backup of the old WAR file and deleting the old WAR file from the Tomcat webapps directory.

---

### Task: Copy new_war to /usr/tomcat

- name: copy new_war -> /usr/tomcat
  import_tasks: copy-new-war.yml

This task imports the tasks defined in the copy-new-war.yml file and executes them. The content of the copy-new-war.yml file is not provided here, but it is likely to contain tasks for copying the new_war file to the /usr/tomcat directory.

---

### Task: Start Tomcat and Check Port 8080

- name: start tomcat & logs
  import_tasks: start-tomcat.yml

This task imports the tasks defined in the start-tomcat.yml file and executes them. The content of the start-tomcat.yml file is not provided here, but it is likely to contain tasks for starting the Tomcat service and checking if port 8080 is up.

---

### Task: Create Backup Directory and Back Up WAR Files

- name: Create backup directory
  file:
    path: "{{ bkp_directory }}"
    state: directory
    mode: '0755'

####  Revisar permisos de la carpeta ROOT

- name: Create backup of WAR files
  copy:
    src: '/usr/share/tomcat/webapps/{{ dest }}.war'
    dest: '{{ bkp_directory }}/{{ dest }}.war.bkp'
    remote_src: true

- name: borrar carpeta old
  file:
    state: absent
    path: '/usr/share/tomcat/webapps/{{ source-file }}/'

- name: borrar .war_old
  file:
    state: absent
    path: '/usr/share/tomcat/webapps/{{ dest }}.war'

These tasks seem to be part of a backup process. The tasks create a backup directory specified by the bkp_directory variable and then back up the existing WAR file by copying it to the backup directory with the .war.bkp extension. Afterward, they delete the old version of the WAR file from the Tomcat webapps directory.

---

### Task: Copy new_war to webapps Directory

- name: Nuevo WAR -->> webapps
  copy:
    src: '{{ home }}/{{ dest }}.war'
    dest: '/usr/share/tomcat/webapps/{{ dest }}.war'
    remote_src: true

This task copies the new_war file from the specified home directory to the Tomcat webapps directory. The destination file name will be the value of the dest variable with a .war extension.

---

### Task: Copy new_war to the home Directory

- name: Copia el Nuevo WAR -->> home
  copy:
    src: '{{ source }}.war'
    dest: '{{ home }}/{{ dest }}.war'

This task copies the new_war file (named as source.war) to the specified home directory with the name specified by the dest variable.

---

### Task: Start Tomcat Service and Check Port 8080

- name: Start Tomcat service
  systemd:
    name: tomcat
    state: started

- name: 8080 state up
  wait_for:
    port: 8080
    timeout: 30

These tasks use the Ansible systemd module to start the Tomcat service and then use the wait_for module to check if port 8080 is up within a timeout of 30 seconds.

---

### Task: Stop Tomcat Service

- name: Stop Tomcat service
  become: yes
  systemd:
    name: tomcat
    state: stopped

This task stops the Tomcat service using the systemd module with the state set to stopped. The become: yes directive indicates that Ansible should elevate privileges to become a superuser (root) to stop the service.

---

### Task: Add SSH Key to Hosts

- name: add key
  host: paja, otros
  become: true
  gather_facts: false

  task:
    - name: creamos directorio de ssh
      file: 
        path: '/home/{{ user }}/.ssh'
        state: directory
        mode: 0700 

    - name: Add Key
      authorized_key:
        user: "{{ user }}"
        key: "{{ lookup('file', './ssh-key.key') }}"
        state: present

These tasks seem to add an SSH key to the specified hosts (paja and otros). It creates the .ssh directory in the user's home directory, sets the correct permissions, and adds the public key from the ssh-key.key file to the user's authorized keys.
