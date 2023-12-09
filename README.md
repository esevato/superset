# superset on proxmox
quick steps to getting Apache Superset up and running on proxmox

1st use the tteck script to initialize a VM, I prefer Debian 12 but I've done it with both the Debiand an Ubuntu VM's

https://tteck.github.io/Proxmox/

This script in particular<br>
```bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/vm/debian-vm.sh)"```

 be sure to tune your vm by following the link at the bottom of the page
 More Info at 
 
 https://github.com/tteck/Proxmox/discussions/1988

 Once your box is up and you have docker installed follow the dockerhub instructiosn except do not pull the latest build, change the script to pull 2.1.0 like this 

```
docker run -d -p 8080:8088 -e "SUPERSET_SECRET_KEY=your_secret_key_here" --name superset apache/superset:2.1.0
```

Modify the following to change from defaults (Just the password will suffice if yer lazy)

```
docker exec -it superset superset fab create-admin \
              --username admin \
              --firstname Superset \
              --lastname Admin \
              --email admin@superset.com \
              --password admin
```

Then initialize your DB

```docker exec -it superset superset db upgrade```
  
Now initialize roles 

```docker exec -it superset superset init```

and finally log in with the user name and password you set up
Login and take a look -- navigate to http://localhost:8080/login/
