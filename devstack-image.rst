Install and setup devstack
==========================

Log in as stack to the server (Password is 'stack')
::
	ssh ubuntu@$server_ip
	
Now update the hostname to the name of your instance
::
	MY_HOST=`curl http://169.254.169.254/latest/meta-data/hostname |awk 'BEGIN {FS=".";}{print $1}'`
	sudo sed -i "s/127.0.0.1 localhost.*$/127.0.0.1 localhost $MY_HOST/" /etc/hosts

Create the 'stack' user and update it in the sudoers file
::
 	sudo adduser stack
 	sudo echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers
 	sudo sed -i "s/^PasswordAuthentication.*/PasswordAuthentication yes/" /etc/ssh/sshd_config
 	sudo service ssh restart

Install git and exit the server
::
  	sudo apt-get install -qqy git
	exit

Now Log in as 'stack', download devstack code and then start 'stack.sh' script.
::
  	ssh stack@$server_ip
  	sudo mkdir /opt/stack
  	sudo chown stack.stack /opt/stack
  	cd /opt/stack
	git clone https://github.com/UTSA-ICS/devstack-juno.git .
	sudo chmod -R 744 *
	cd devstack
	sudo apt-get install --reinstall python-setuptools
	sudo easy_install --upgrade pip
	./stack.sh

Ensure that there are no errors and this pases successfully. 
If not then please check out the "Issues" section for further debugging.
https://github.com/UTSA-ICS/devstack-101/issues
