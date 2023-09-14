# Ansible-playbook
🔰Write an Ansible PlayBook that does the following operations in the managed nodes: 🔹 Configure Docker 🔹 Start and enable Docker services 🔹 Pull the httpd server image from the Docker Hub 🔹 Run the docker container and expose it to the public 🔹 Copy the html code in the/var/www/html directory and start the web server
To accomplish the specified operations on managed nodes using Ansible, you can create the following Ansible playbook:

yaml
Copy code
---
- name: Configure and Run Docker Container
  hosts: managed_nodes  # Replace with the name of your target group or host(s)
  become: yes
  tasks:
    - name: Install Docker
      apt:
        name: docker.io  # Install Docker package (for Ubuntu/Debian-based systems)
        state: present
      when: ansible_distribution == 'Ubuntu'  # Use 'yum' for CentOS/RHEL-based systems

    - name: Start and enable Docker service
      service:
        name: docker
        state: started
        enabled: yes

    - name: Pull the httpd server image from Docker Hub
      docker_image:
        name: httpd:latest
        source: pull

    - name: Create /var/www/html directory
      file:
        path: /var/www/html
        state: directory

    - name: Copy HTML code to /var/www/html directory
      copy:
        src: /path/to/your/index.html  # Replace with the path to your HTML file
        dest: /var/www/html/index.html

    - name: Run the Docker container
      docker_container:
        name: my_httpd_container  # Name for the Docker container
        image: httpd:latest
        ports:
          - "80:80"  # Expose container port 80 to host port 80
        volumes:
          - /var/www/html:/usr/local/apache2/htdocs/
        state: started
Make sure to replace managed_nodes with the appropriate host group or individual host(s) where you want to perform these operations. Additionally, replace /path/to/your/index.html with the actual path to your HTML file that you want to serve.

This playbook performs the following operations:

Installs Docker on the managed nodes, depending on whether they are Ubuntu/Debian-based or CentOS/RHEL-based systems.

Starts and enables the Docker service.

Pulls the latest httpd server image from Docker Hub.

Creates the /var/www/html directory on the managed nodes.

Copies your HTML code to the /var/www/html directory on the managed nodes.

Runs a Docker container named my_httpd_container, using the httpd:latest image, mapping port 80 in the container to port 80 on the host, and mounting the /var/www/html directory into the container. This starts the Apache web server in the container and serves the HTML content you copied.

To execute this playbook, save it to a file (e.g., docker_setup.yml) and run it using the ansible-playbook command:

bash
Copy code
ansible-playbook docker_setup.yml
Ensure that you have Ansible configured properly and that you have SSH access to the managed nodes. Adjust the playbook as needed to match your specific environment and requirements.
