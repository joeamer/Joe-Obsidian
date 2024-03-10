`#!/bin/bash` 
`for name in $(ls /home/bob/assets/)` 
`do` 
`echo "Cleaning file $name" >> /home/bob/clean.log rm -rf /home/bob/assets/$name`
`done`

`#!/bin/bash` 
`if [ $(systemctl is-enabled httpd.service) = disabled ] then` 
	`echo "httpd.service is disabled. Enabling httpd.service."` 
`systemctl enable httpd.service` 
`else` 
	`echo "httpd.service is enabled."` 
`fi`

``