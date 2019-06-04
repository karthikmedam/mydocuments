## CHEF
scp command:
- 
scp filename ec2-user@IP of other machine:~

Installation of Chef-server
-
* cat /etc/issue --> it will show which os is using
* cat /etc/hosts --> where you will keep all the Ip of chef server chefworkstation and chefnode
* example:
	* 192.168.0.10 chefserver
	* 192.168.0.11 chefworkstation
	* 192.168.0.12 chefnode 
* ping chefworkstation --> to check if you are able to communicate with chef workstation
* ping chefnode --> to check if you are able to communicate with chef node
* iptables -L --> to see if IP table is configured or not 
* copy server url from internet and wget <url>
* install the package --> rpm -ivh chef-server.......
* --> after server is installed you have to configure the server
* sudo chef-server-ctl reconfigure
* sudo chef-server-ctl status
* cd /etc/chef-server/ ---> you will find chef certificate

Installation of workstation
-
* ifconfig
* cat /etc/hosts --> where you will keep all the Ip of chef server chefworkstation and chefnode
* example:
	* 192.168.0.10 chefserver
	* 192.168.0.11 chefworkstation
	* 192.168.0.12 chefnode 
* ping chefserver --> to check if you are able to communicate with chef-server
* ping chefnode --> to check if you are able to communicate with chefnode
* iptables -L --> to see if IP table is configured or not
* copy url from internet and wget <url>
* install the package --> rpm -ivh chef........
* if you go to /etc/chef-server you will find 1) admin.pem 2) chef-validator.pem and 3) chef-webui.pem. you have to COPY those files in to the chef   workstation.
* examples:
	* scp admin.pem ec2-user@ip:~
	* scp chef-validator.pem ec2-user@ip:~
	* scp chef-webui.pem ec2-user@ip:~
* now create the folder in home directory --> mkdir .chef
* copy all the .pem file inside that .chef folder
* now you have to configure knife so that workstation will be able to communicate with chef-server.
* kinfe ssl fetch 
* knife ssl ckeck 
* sudo knife configure -i 
* enter the chef-server url : https://chefserver.example.com:443
* enter the name of the new user: ramhari (sudo useradd ramhari)
* enter the existiong admin name: admin
* enter the location of the exisitng admin's private key: ~/.chef/admin.pem
* enter the location of the validation key: ~/.chef/chef-validator.pem
* enter the path of the chef repository: leave it blank
* Please enter the password for the new user: sanimabank (sudo passwd ramhari)
* ls -latr --> you will see the trusted_cets which is certificate
* knife client list --> it will show the list
	* chef-validator
	* chef-webui
* knife user list --> it will show ec2-user and ramhari

* NOTE: now the next step is to configure the node and tell the node to communicate with chef-server using the certificate. 

Installation of chefnode
- 
* cat /etc/hosts --> where you will keep all the Ip of chef server chefworkstation and chefnode
* example:
	* 192.168.0.10 chefserver
	* 192.168.0.11 chefworkstation
	* 192.168.0.12 chefnode 
* ping chefserver --> to check if you are able to communicate with chef-server
* ping workstation --> to check if you are able to communicate with workstation
* copy the url from the internet adn wget <url> (installation of chef not chef-server)
* rpm -ivh chef............
* make a directory in : sudo mkdir /etc/chef
* now copy the chef-validator.pem file from chef-server: scp chef-validator.pem ec2-user@ip:~ 
* now copy the chef-validator.pem to chef folder: sudo cp chef-validator.pem /etc/chef
* sudo knife ssl fetch -S https://chefserver.example.com
* now if you goto the .chef file in home directory. you will find trusted_certs
* sudo knife ssl check -S https://cheserver.example.com
* you will get the msz --> successfully verified certificates from chefserver.example.com
* cd /ect/chef
* vim client.rb
* content will be:
	* log_level :info 	  
	* log_location STDOUT
	* chef_server_url "https://chefserver.example.com:443"
	* trusted_certs_dir "~/.chef/trusted_certs"
* now you need to join this node to the server.
* chef-client -S https://chefserver.example.com -K /etc/chef/chef-validator.pem
* now goto the workstation and execute the following commands:
* knife client list --> you will see chef-validator, chef-webui and chefnode.example.com. earlier there were only two clints.
* now copy the public ip of chef-server in url place you will able to open chef server UI.
* Login username: admin Password: P@sswOrd1 --> you have to change the password.
* chef generate cookbook <cookbook name> or knife cookbook create <cookbook name>
* it will make a cookbook inside /var/chef
* if you go inside the cookbook --> you will see the whole structure created by the chef.
* cd cecipe --> default.rb--> vi deafult.rb>> and write small file resource save and exit.
* knife cookbook test <cookbook name> --> check the syntax of the cookbook.
* knife cookbook upload <cookbook name> --> uploading the cookbook into the chefserver.
* you can check from command line tool also. --> sudo knife cookbook list
* now from the UI of chef server you can drag and drop the recipe to the runlist and goto the chefnode and execute following command.
* chef-client --> it will execute that recipe to the node.
	
	
	
