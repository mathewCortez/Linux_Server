
## i. The IP address and SSH port so your server can be accessed by the reviewer.

The public IP address is 35.167.25.97 and the SSH port is 2200

## ii. The complete URL to your hosted web application.

None yet

## iii. A summary of software you installed and configuration changes made.

The first thing I did was going from vagrant to signing in as root into AWS. Then I created the grader user using the sudo adduser grader. Then I added grader to the sudo group with the usermod -aG sudo grader command. I then added grader to the sudo group in the /etc/sudoers group. I added my IP to the end of 127.0.0.1 localhost line in the /etc/hosts file so I did not have the "Sudo: unable to resolve host error". I then changed the ssh port in /etc/ssh/sshd_config to listen to port 2200. I spent a day trying to login to grader because I was getting a public key error, so I just had to move the authorized_keys file to the grader .ssh directory to fix that and I ended up signing in from my local computer to AWS just to see if I could do that. After that I was not able to sign back into root, but that is supposed to happen so I did not think anything was wrong there. Next, in grader I ran sudo apt-get update and sudo apt-get upgrade to update everything. I configured the the timezone to be UTC using the sudo dpkg-reconfigure tzdata command and configured the firewall and enabled it.  I used the commads "sudo ufw allow 2200" for the ssh port, "sudo ufw allow http" which defaults to port 80, and the ntp "sudo ufw allow 123/tcp".  I then ran sudo ufw status to check the ports.

Then I added this line "config.vm.network :forwarded_port, guest: 80, host: 8080" even though I was not getting to AWS through vagrant, and then I downloaded apache2. The index.html file for apache2 displayed on the amazon environment IP.

Next I downloaded mod-wsgi using sudo apt-get install libapache2-mod-wsgi.  I added the WSGIScriptAlias / /var/www/html/myapp.wsgi line to the 000-default.conf file.  I wrote this in myapp.wsgi to test if it works
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
After this worked I installed PostgreSQL using sudo apt-get install postgresql and ran sudo nano pg_hba.conf from within the postgresql directory to see if allow remote connections were blocked, and they were.

Next I worked on creating a flask app first by moving to cd /var/www and running sudo mkdir catalog, then cd catalog, making another catalog directory within catalog.  Then I cd into FlaskApp and created two subdirectories using sudo mkdir static templates. I added __init__.py and catalog.wsgi with the contents from the https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps website.  This all worked well so I started to create the catalog user.  Fan the commands sudo -u postgres psql -> CREATE ROLE catalog LOGIN PASSWORD 'catalog' -> \q -> createdb catalog -> exit -> sudo adduser catalog (with password catalog) -> logged in with sudo -u catalog psql -> also changed line in pg_hba.conf and changed all to md5 as I saw in the udacity forums

Next I downloaded git using sudo apt install git -> git config --global user.email "mcortez121@gmail.com" -> git config --gloabl user.name "mathewCortez" -> then I had to get ssh keys and connect that with my account -> ssh-keygen -t rsa -b 4096 -C "mcortez121@gmail.com" and ssh-add ~/.ssh/id_rsa then copied the key to my clipboard to add to my github -> git clone https://www.github.com/mathewCortez/FSWD-Item-catalog into the catalog directory.

Then I downloaded all the packages I needed for the catalog project to run inside the virtual environment -> pip install bleach httplib2 request oauthclient sqlalchemy python-psycopg2 -> the python-psycopg2 did not downlaod so I used sudo apt-get install python-psycopg2 and it worked.

It said I didnt have the packages so installed sqlalchemy outside of the virtual env and then ran this command  -> ln -s /usr/lib/python2.7/dist-packages/sqlalchemy $VIRTUAL_ENV/lib/python2.7/site-packages
I changed my sqlite to postgressql in the catalog files to postgresql://catalog:catalog@localhost/catalog

Ran these commands in psql - from https://discussions.udacity.com/t/no-module-named-sqlalchemy-white-running-project-in-linux/203592/26
ALTER USER catalog CREATEDB;
CREATE DATABASE catalog WITH OWNER catalog;
Connect to db: 
\c catalog
GRANT ALL ON SCHEMA public TO catalog;
quit and exit: 
\q
exit

Than I ran python database_setup.py and lotsofmenus.py 
This displayed the app on the localhost but the login was not working
I changed authorized Javascript orgins to include the AWS IP.  

I then took out if __name__ = '__main__' in the catalog.py file so the secret key would be generated which is needed for the Google sign in to work.  I changed the contents in the client_secrets file to reflect the changes make to the authorized urls.



## iv. third-party resources you made use of to complete this project.

I downloaded wget to help find the source of some problems
wget -O - http://localhost:8080/index.html - I was able to get the apache2 index.html file with this command so I do not understand why it is not displaying on that URL.





