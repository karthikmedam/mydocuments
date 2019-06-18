
## CHEF

Chef server:
-
You can direct use hosted chef-server. For that you have to login to https://manage.chef.io and create an account.

Chef server installation in Ubuntu
-
* Lunch at least t2 medium ubuntu 16.04 server 
* sudo apt-get update
* wget https://packages.chef.io/files/stable/chef-server/12.19.31/ubuntu/16.04/chef-server-core_12.19.31-1_amd64.deb
*  ls
* sudo dpkg -i chef-server-core_12.19.31-1_amd64.deb 
* sudo chef-server-ctl reconfigure
* sudo chef-server-ctl status
* sudo chef-server-ctl test (optional)
* sudo chef-server-ctl service-list

create user
-
* sudo chef-server-ctl user-create ramharig Ram Ghimire ghimire.ramhari87@gmail.com sanimabank --filename /home/ubuntu/ramharig.pem
 	* where,
	1. ramharig —> is username
	2. Ram Ghimire —> full name of the user
	3. Email —> is user’s email
	4. sanimabank —> is the password
	5. /home/ubuntu/ramharig.pem —> create ramharig.pem key in home  

Create org
-
* sudo chef-server-ctl org-create rhythm "rhythm.com" --association_user ramharig -f ~/rhythm-validator.pem
	* where,
	1. rhythm —> organization short name
	2. rhythm.com —> is organization long name
	3. rhythm-validator.pem —> is organization’s key

GUI for chef-server
-
* sudo chef-server-ctl install opscode-manage
* chef-server-ctl reconfigure 
* sudo chef-manage-ctl reconfigure —>hit ‘q’ and  type yes to agree the license 
* sudo chef-server-ctl install opscode-push-jobs-server
* sudo chef-server-ctl reconfigure
* sudo opscode-push-jobs-server-ctl reconfigure
* sudo chef-server-ctl install opscode-reporting
* sudo chef-server-ctl reconfigure
*  sudo opscode-reporting-ctl reconfigure —> hit q and type yes to agree the license

workstaion:
-
* Download the chef-dk in local machine or scp to the workstaion
* unzip the file
* cd chef-repo

Bootstrap command (Linux)
-
* knife bootstrap 3.87.185.66 --ssh-user ec2-user --sudo -i ~/aws.pem -N nodenew2 -y
	* Where, 
	1. 3.87.185.66 —> is public ip of the node machine
	2. —ssh-user ec2-user --sudo -i —> is commands
	3. ~/ramhari.pem —> I have send my aws.pem file to the workstation’s home using scp commands. (EXAMPLE: aws.pem is my pem key to connect aws 		      machine which you create while lunching the ec2-instance)
        4. -N nodenew2 -y —> is commands
