# aca metemos la docu

Requisitos previos
Tener Ansible instalado en el equipo local.
Acceso SSH a los servidores A, B y C.
Pasos a seguir
Crear el archivo main.yml:

#Crea un nuevo archivo con el nombre main.yml y copia el siguiente contenido:

- [ link ] \(ansible-project-deploy/roles/main.yml)- name: Seleccion del HOST
  hosts: A, B, C
  gather_facts: false
  vars_prompt:
    - name: selected_group
      prompt: "Select a group (A-B  //  C): "
      private: false
      default: "A-B"

  tasks:
    - name: Display group
      debug:
        var: selected_group

  roles:
    - role: tomcat
      when: "'A-B' in selected_group"
      host: A

    - role: tomcat
      when: "'A-B' in selected_group"
      host: B

    - role: tomcat
      when: "'C' in selected_group"
      host: C
Crear el archivo backup.yml:

Crea un nuevo archivo con el nombre backup.yml y copia el siguiente contenido:

- name: Create backup directory
  file:
    path: "{{ bkp_directory }}"
    state: directory
    mode: '0755'

- name: Create backup of WAR files
  copy:
    src: '/usr/share/tomcat/webapps/{{ dest }}.war'
    dest: '{{ bkp_directory }}/{{ dest }}.war.bkp'

- name: borrar carpeta old
  file:
    state: absent
    path: '/usr/share/tomcat/webapps/{{ source-file }}/'

- name: borrar .war_old
  file:
    state: absent
    path: '/usr/share/tomcat/webapps/{{ dest }}.war'
Crear el archivo copy-new-war.yml:

Crea un nuevo archivo con el nombre copy-new-war.yml y copia el siguiente contenido:

- name: Nuevo WAR -->> webapps
  copy:
    src: '{{ home }}/{{ dest }}.war'
    dest: '/usr/share/tomcat/webapps/{{ dest }}.war'
Crear el archivo copy-war.yml:

Crea un nuevo archivo con el nombre copy-war.yml y copia el siguiente contenido:

- name: Copia el Nuevo WAR -->> home
  copy:
    src: '{{ source }}.war'
    dest: '{{ home }}/{{ dest }}.war'
Crear el archivo start-tomcat.yml:

Crea un nuevo archivo con el nombre start-tomcat.yml y copia el siguiente contenido:

- name: Start Tomcat service



---

# examples
## asd
### asd
- [links](./role/tomcat/task/buckup.yml)

listas 
- a
- b 
- c 
  - c.a
    - c.b

<details>
<summary>User's machine</summary>

- Ansible Install
- Internet Access
- Access to the 6 nodes
- Access to the repository
</details>

``` colorcito verde ```
