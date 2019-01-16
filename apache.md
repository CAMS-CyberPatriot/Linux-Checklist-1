# Wando Team 2oe Apache Checklist

## Notes
Assume root permissions are needed for most commands. You can use `sudo` or become root with `su`.

## Checklist
1. Update `apache2` to the latest version
  
  `$ apt-get install apache2`
1. Start the Apache service
  1. If using systemd
  
    `$ service apache2 start`
  1. If using Upstart
  
    `$ /etc/init.d/apache2 start`
1. Audit the existing Apache installation
  1. See what the website currently looks like
  
    `$ wget 127.0.0.1`
  1. Look through and backup the `/var/www` directory
  
    `$ ls -l /var/www`
    
    `$ rsync -avzh /var/www $destination_dir
  1.
