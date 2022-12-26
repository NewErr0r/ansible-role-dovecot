<h1 align='center'>Автоматизация развёртывания веб-приложение для управления почтовыми ящиками и доменами в Postfix 'PostfixAdmin'</h1>

<p>
    <strong>Шаг 1. </strong> Создание playbook для запуска роли
</p>
<p><i>Пример:</i></p>

    ---
    - name: Deploy PostfixAdmin
      hosts: all 
      become: true 

      roles: 
        - ansible-role-postfixadmin

<p>
    <strong>Шаг 2. </strong> Склонировать роль в дирректорию с playbook:
</p>

  <pre>git clone https://github.com/NewErr0r/ansible-role-postfixadmin.git</pre>

<p>

<p>
    <strong>Пример переопределяемых переменных для playbook. </strong>
</p>
<pre>
#System preparation
hostname: 'dovecot.champ.first'
timezone: 'Europe/Moscow'

#MariaDB
mariadb_root_password: "P@ssw0rd"

#PostfixAdmin
path_download_postfixadmin: /root
potsfixadmin_database_name: 'postfix'
postfixadmin_database_username: 'postfix'
postfixadmin_database_username_password: 'postfix123'

#Creating a directory for postfixadmin
dir_postfixadmin: /usr/share/nginx/html/postfixadmin
</pre>

<p>
    <strong>Шаг 3. </strong> Запуск playbook:
</p>

  <pre>ansible-playbook -i inventory/hosts playbook.yml</pre>

<p>

<p>
    <strong>Шаг 4. </strong> Запускаем браузер и вводим адрес http://<IP-адрес сервера>/postfixadmin/public/setup.php:
</p>
