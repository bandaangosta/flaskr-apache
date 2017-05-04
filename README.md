This is the Flaskr tutorial application from the Flask documentation (as of version 0.12, found at http://flask.pocoo.org/docs/0.12/tutorial/), written from scratch.
Additional files were added to allow for this Flask application to run using Apache 2.4 + mod_wsgi. Implementation was successful on a Raspberry Pi Zero hooked to the internet through a regular home ISP router.

Some design considerations (as this is a tutorial, you may have particular interest in this details):
* Flaskr app is implemented using a Python virtual enviroment (virtualenv). This is the recommended way of doing things, as usual.
* A separate Apache site configuration file is used for the Flaskr app setup. 
* The home router blocks outgoing connections on port 80, so a different port was used. 1096 in this case, but any other non-reserved number should do. Don't forget to open such port in the router firewall and forward it to the server in use (Raspberry Pi in this case).

## Installation
This steps assume you wish to run the app with Apache + wgsi. If you just want to test it, just download the code and follow the usual _flask run_ steps.
* Install Apache in your server (doh!)
* Install mod_wsgi module. In Debian-based distributions:
  * (sudo) apt-get install libapache2-mod-wsgi
* In /etc/apache2/ports.conf add your port number:
  * Add "Listen 1096" 
* Create a site configuration file. E.g., /etc/apache2/sites-available/005-flask-test.conf. Used the one in apache/ as a reference.
* Enable the new site:
  * sudo a2ensite 005-flask-test
*  Get a copy of the source code by using _git clone_ or similar into the folder you choose. In this example, _/var/www/_ was used. Edit the flaskr.wsgi and site config file accordingly.
* In the new _<app root>/flaskr/_ folder create a virtual environment:
  * virtualenv venv
* Activate the virtual environment and install the app:
  * source /venv/bin/activate
  * pip install . (_pip install --editable ._ if you want to change the code in place)
* Change file permissions to ensure webserver user can write to the flaskr.db database file (file and containing folder!)
* Restart web server!
  * sudo service apache2 restart
* In your web browser head to http://<your address>:1096
