# Instalar postgresql

$ sudo apt-get install postgresql

sudo service postgresql start|stop|restart

PostgreSQL, to use a local database

After installation you will need to create a postgres user: by default the only user is postgres, and Odoo forbids connecting as postgres.

on Linux, use your distribution's package, then create a postgres user named like your login:

$ sudo su - postgres -c "createuser -s $USER"

# Instalar odoo

wget -O- https://raw.githubusercontent.com/odoo/odoo/8.0/odoo.py | python

sudo apt-get install libpq-dev

pip install -r requirements.txt