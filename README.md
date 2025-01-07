first clone this repository using 
step 1:-
    git clone https://github.com/brijesh7990/pythonproject

step 2:-
    poetry config virtualenvs.in-project true
    poetry install

step 3:-
    cd /etc/systemd/system && sudo nano pythonproject.socket

    #paste given code.
    
    [Unit]
    Description=gunicorn socket
    
    [Socket]
    ListenStream=/run/gunicorn.sock
    
    [Install]
    WantedBy=sockets.target

step 4:-
    cd /etc/systemd/system && sudo nano gunicorn.service

    #paste given code
    
    [Unit]
    Description=pythonproject instance to serve python project
    After=network.target
    
    [Service]
    User=brijesh
    Group=www-data
    WorkingDirectory=/home/brijesh/Documents/pythonproject
    ExecStart=/home/brijesh/Documents/pythonproject/.venv/bin/gunicorn  --bind unix:/run/pythonproject.sock --access-logfile /var/log/pythonproject/access.log --error-logfile /var/log/pythonproject/error.log project.wsgi:application
    Environment="PATH=/home/brijesh/Documents/pythonproject/.venv/bin"
    
    [Install]
    WantedBy=multi-user.target

step 5:-
    sudo systemctl start pythonproject.socket
    sudo systemctl enable pythonproject.socket
    #sudo systemctl enable pythonproject.service
    sudo systemctl status pythonproject.service

    sudo chown brijesh:www-data /run/pythonproject.sock
    sudo chown brijesh:www-data /run/pythonproject.sock
    
    sudo systemctl restart pythonproject.socket
    sudo systemctl restart pythonproject.service

    sudo systemctl enable pythonproject.socket
    sudo systemctl enable pythonproject.service

    

step 6:
    #verify a log
    sudo journalctl -u pythonproject.service
    sudo journalctl -u pythonproject.socket

step 7:-
    #verify a socket connection
    sudo ss -lx | grep pythonproject

step 8:-
    sudo systemctl daemon-reload



