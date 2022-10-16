This is used in the demonstration of development of Ansible Playbooks.

  Below are the steps required to get this working on a base linux system.

  - Install all required dependencies
  - Start Web Server

## 1. Install system dependencies:
###     ---> DONE, both web and db, verified w/yamllint and '--check'

  Python and its dependencies

    apt-get install -y python3 python3-setuptools python3-dev build-essential python3-pip python3-mysqldb


## 2. Install application dependencies:
###     ---> DONE, both web and db, verified w/yamllint and '--check'

Install python dependencies

    pip3 install configparser
    pip3 install flask
    pip3 install flask-sqlalchemy

## 3. Download the application:
###     ---> DONE, web only, verified w/yamllint and '--check'

    git clone https://github.com/jrrickerson/flask-guestbook /opt/flask-guestbook

## 4. Start Web Server
##      4a.  * Note, The "kill" step was pre-done by JR
##      ----> Trying different method using builtin instead of shell
        Ensure the application is not already running
        kill $(ps -ef |grep -v grep | grep -w flask |awk '{print $2}')
    
##      4b.  Done, I think - passes yamllint, ansible-lint, and --check
        Start web server in a background process
        FLASK_APP=/opt/flask-guestbook/app.py nohup flask run -h 0.0.0.0 -p 8080 &

## 5. TODO: Test
    List messages
    curl http://<IP>:8080/signatures

    Write a message
    curl -X POST http://<IP>:8080/signatures?message="my message"

# Add a mysql database

## 1. Install and Configure Database

### Install MySQL database
###     ---> Done (passes yamllint, ansible-lint and '--check')
        apt-get install -y mysql-server mysql-client

### Allow any host to connect 
###     ---> Done (maybe???).  (passes yamllint, ansible-lint and '--check')
    
## Trying different method using builtin instead of shell / sed
        sed -i "s/.*bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf


## 2. Start Database Service
###     ---> Done (maybe???).  (passes yamllint, ansible-lint and '--check')
        Start the database service

        service mysqld start

        - Create database and database users

        # mysql -u <username> -p

        mysql> CREATE DATABASE guestbook;
        mysql> GRANT ALL ON *.* to db_user@'%' IDENTIFIED BY 'Passw0rd';

### TODO
###     ---> Haven't figured out how to do this.
  - Configure database credentials and parameters for the app
        The app looks for a configuration file at '/opt/guestbook/vars.ini'
