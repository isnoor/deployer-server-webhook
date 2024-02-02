## Project config
Please make sure project already clone from git with ssh authentication.


## Install python3 & venv
sudo apt install python3
apt-get install python3-venv

## Install Flask & unicorn
python3 -m venv venv

source venv/bin/activate
pip install Flask
pip install gunicorn

## Modify app.py
Copy app.py.example to app.py. Modify file app.py

## Create gunicorn service
Open runasservice.example. Or this command below.
````
sudo nano /etc/systemd/system/gunicorn.service
````
````
[Unit]
Description=Gunicorn instance to serve application
After=network.target

[Service]
User=<userexecutor>
Group=<userexecutor>
WorkingDirectory=/home/<userproject>/deployer
Environment="PATH=/usr/local/bin:/usr/bin:/bin"
Environment="PYTHONPATH=/home/<userproject>/deployer/venv/bin/"
ExecStart=/home/<userproject>/deployer/venv/bin/gunicorn --workers 5 --bind 0.0.0.0:5000 app:app
ExecReload=/bin/kill -s HUP $MAINPID
KillMode=mixed
TimeoutStopSec=5
PrivateTmp=true

[Install]
WantedBy=multi-user.target
````

````
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
````
