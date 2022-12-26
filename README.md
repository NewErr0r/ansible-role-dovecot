<h1 align='center'>Автоматизация развёртывания веб-приложения для управления почтовыми ящиками и доменами в Postfix 'PostfixAdmin'</h1>

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
    <strong>Список переопределяемых переменных для playbook. </strong>
</p>
<pre>
#System preparation
hostname: 'dovecot.champ.first'
timezone: 'Europe/Moscow'
<br>
#MariaDB
mariadb_root_password: "P@ssw0rd"
<br>
#PostfixAdmin
path_download_postfixadmin: /root
potsfixadmin_database_name: 'postfix'
postfixadmin_database_username: 'postfix'
postfixadmin_database_username_password: 'postfix123'
<br>
#Creating a directory for postfixadmin
dir_postfixadmin: /usr/share/nginx/html/postfixadmin
</pre>

<p>
    <strong>Шаг 3. </strong> Запуск playbook:
</p>

  <pre>ansible-playbook -i inventory/hosts playbook.yml</pre>

<p>

<p>
    <strong>Шаг 4. </strong> Запускаем браузер и вводим адрес http://'IP-адрес сервера'/postfixadmin/public/setup.php:
</p>
<p>
Начнется процесс проверки конфигурации и установки портала PostfixAdmin. После ее окончания мы увидем окно с результатами — проверяем, чтобы не было ошибок и предупреждений<br>
... после чего, вводим дважды пароль и генерируем хэш, кликнув по Generate password hash
</p>

<p>
    <strong>Шаг 5. </strong> После перезагрузки страницы копируем хэш:
</p>
<p>
Подключаемы по SSH к серверу, открываем конфигурационный файл 
</p>
<pre>
vi /usr/share/nginx/html/postfixadmin/config.local.php
</pre>
<p>
И добавляем строчку:
</p>
<pre>
...
$CONF['setup_password'] = '7a8e14...c26';
</pre>
<i>где '7a8e14...c26' — скопированный хэш.</i>

<p>
    <strong>Шаг 6. </strong> Обновляем страницу в веб-браузере, добавляем суперпользователя PostfixAdmin:
</p>
<p>
И переходим в браузере на страницу http://'IP-адрес сервера'/postfixadmin/public/
</p>
<p>
Вводим логин и пароль для созданного пользователя
</p>

<p>
    <strong>Шаг 7. </strong> Автоматизация развёртывания и настройки Postfix. Настройка Dovecot: https://github.com/NewErr0r/ansible-role-postfix-dovecot.git
</p>
