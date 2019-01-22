Provisioning a new site
    =======================
    ## Required packages:
    * nginx
    * Python 3.6
    * virtualenv + pip
    * Git

eg, on Ubuntu:
        sudo add-apt-repository ppa:fkrull/deadsnakes
        sudo apt-get install nginx git python36 python3.6-venv

## Nginx Virtual Host config
    * see nginx.template.conf
    * replace SITENAME with, e.g., staging.my-domain.com

## Systemd service
    * see gunicorn-systemd.template.service
    * replace SITENAME with, e.g., staging.my-domain.com

export SITENAME=sln8.hopto.org
sed "s/SITENAME/sln8.hopto.org/g" source/deploy_tools/nginx.template.conf | sudo tee /etc/nginx/sites-available/sln8.hopto.org
sudo ln -s ../sites-available/sln8.hopto.org /etc/nginx/sites-enabled/sln8.hopto.org
sed "s/SITENAME/sln8.hopto.org/g" source/deploy_tools/gunicorn-systemd.template.service | sudo tee /etc/systemd/system/gunicorn-sln8.hopto.org.service

sudo systemctl daemon-reload
sudo systemctl reload nginx
sudo systemctl enable gunicorn-sln8.hopto.org
sudo systemctl start gunicorn-sln8.hopto.org

## Folder structure:
    Assume we have a user account at /home/username
    /home/username
    └── sites
        └── SITENAME
             ├── database
             ├── source
             ├── static
             └── virtualenv
