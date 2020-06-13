## CI Infrastructure Automation

### Prerequisites

1. Install ansible.
2. Add your SSH public key to target server.
2. Edit /etc/ansible/hosts file to add follow content:
    ```
    [jenkins]
    IP:PORT

    [sonarqube]
    IP:PORT

    [gitlab]
    IP:PORT
    ```

### Install Jenkins

1. `ansible-galaxy install geerlingguy.jenkins`.
2. `ansible-galaxy install geerlingguy.docker`.
3. `ansible-galaxy install geerlingguy.git`.
4. `ansible-playbook jenkins.yml -l jenkins`.
5. 重启jenkins，然后安装插件.
6. 登录目标服务器并执行以下命令:
    * `chmod 777 /var/run/docker.sock`.
    * `chown -R root /var/lib/jenkins`.
    * `chown -R root /var/log/jenkins`
7. 安装jenkins插件：
    * SonarQube Scanner
    * SSH Agent
8. 生成ssh key，`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`。
9. 将生成的private key添加为jenkins的credential。

### Install SonarQube

1. `ansible-galaxy install lrk.sonarqube`.
2. `ansible-playbook sonarqube.yml -l sonarqube`.
3. 生成sonar token并在jenkins添加credential。

### Install Gitlab

1. `ansible-galaxy install geerlingguy.gitlab`.
2. `ansible-playbook gitlab.yml -l gitlab`.
3. `vim /var/opt/gitlab/gitlab-rails/etc/gitlab.yml`.
    ```yaml
    gitlab:
        host: <域名>
        port: 443
        https: true
    ```
4. `gitalb-ctl restart`.

### Install Nginx

1. `ansible-galaxy install geerlingguy.nginx`.
2. `ansible-playbook nginx.yml -l nginx`.