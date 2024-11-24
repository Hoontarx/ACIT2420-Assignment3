# ACIT2420-Assignment3

## Task 1 - Creating New System User
We are going to start by creating a new system user so we can store and run the generate_index script file inside their home directory in their bin folder.

First we are going to create the new system user using useradd, specifying their home directory and an appropriate shell for the new system user.

You can run the following:
```bash
sudo useradd -r -m -d /var/lib/webgen -s /usr/sbin/nologin
```

Now that we have the new user, we are going to add the necessary file structure to their home directory:
```bash
sudo mkdir -p /var/lib/webgen/bin
sudo mkdir -p /var/lib/webgen/HTML
```

We will now copy the script file into the webgen users home directory in their bin folder:
```bash
sudo cp /path/of/script/file /var/lib/webgen/bin
```

Now that webgens home directory is fully set up, we need to change the ownership to webgen:
```bash
sudo chown -R webgen:webgen /var/lib/webgen
```

## Task 2 - Creating .service & .timer Files
For the second task, we are going to be creating a .service file and a .timer file that will run our script automatically everyday at 5am.

First lets create the generate-index.service file:
```bash
nvim generate-index.service
```

Now inside the .service file we will add the following:
```
[Unit]
Description=Running the generate_index script
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
User=webgen
Group=webgen
ExecStart=/var/lib/webgen/bin/generate_index

[Install]
WantedBy=multi-user.target
```

Next we will create the generate-index.timer file:
```
nvim generate-index.timer
```

Now inside the .timer file we will add the following:
```
[Unit]
Description=Running the generate_index script everyday at 5 am

[Timer]
OnCalendar=*-*-* 5:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

>[!NOTE]
>THIS IS WHERE I HAVE LEFT OFF, STILL NEED TO MOVE/COPY FILES TO /etc/systemd/system AND COMPLETE THE REST

Now that we have both the .service and .timer files, we need to put them in the correct directory:
```bash
sudo cp generate-index.service /etc/systemd/system/
sudo cp generate-index.timer /etc/systemd/system/
```

Then we will reload systemd:
```bash
sudo systemctl daemon-reload
```

Lastly, we will start and enable the service:

## Task 3 - Setting Up nginx Config
## Task 4 - Setting Up ufw
