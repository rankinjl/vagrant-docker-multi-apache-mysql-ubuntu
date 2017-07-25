# vagrant-docker-multi-apache-mysql-ubuntu
Use Vagrant to set up multiple Docker containers on a private network: one Apache WebContainer with a form on a web page and one MySQL DBContainer that receives and stores this form data.

Edited by Jessica Rankins on 7/24/2017

Create Ubuntu machine via Vagrantfile on Windows host,
vagrant up, and use Vagrantfile and Dockerfiles in Linux environment


GOALS:
- Use Vagrant and Docker to configure multiple interacting Docker 
		containers in a Vagrantfile	on a linux machine (Ubuntu)
- Demonstrate Infrastructure as code principles (script configures 
		and provisions environments to ensure environment parity)
- Networking: 
		Bridge network for containers created,
		forwarded ports to host from VM from Web container: 
		80 to 8080 to 8080 for apache webserver demonstration on linux
		
PREREQUISITES: ON WINDOWS HOST COMPUTER, HAD TO:
- Install VirtualBox (any version should work)
- Install Git for Windows (version 2.12.2.windows.1)
- Install Vagrant (version 1.9.4)
	
TO EXECUTE:
- cd to directory with Vagrantfile
- make sure you have a "Docker" folder with a public-html folder, 
		DBDockerfile, WebDockerfile, Vagrantfile
- ```vagrant up```
- ```vagrant ssh```
- ```cd /vagrant/Docker```
- ```docker network create --subnet=172.18.0.0/16 my-bridge-network```
- ```vagrant up```
- ```docker exec webcontainer service apache2 restart```
- ```docker exec dbcontainer service mysql restart```
- See example web page in Windows host browser at 127.0.0.1:8080
	
- When done, ```vagrant destroy``` in /vagrant/Docker to get rid of containers
- ```exit``` from vagrant ssh
- ```vagrant destroy``` to get rid of Ubuntu VM
	
SOME USEFUL MYSQL COMMANDS:
- ```vagrant docker-exec DBContainer -- mysql -uroot -p'rootpass' -e "command;[command;...]"```
		(default username = root, password = rootpass)
- "SHOW DATABASES;" to list available databases
- "USE databasename;" to change databases
- "CREATE DATABASE databasename;" to create database 
- "DROP DATABASE databasename;" to delete database
- "SHOW tables;" to see available database tables
- "DESCRIBE tablename;" to see overview of table (field, type, etc)
- "CREATE TABLE tablename (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
		column1 TYPE, column2 TYPE,..., columnN TYPE);" to create
		a table in the current database
    - columnJ is the label of the column
    - TYPE can be things like: VARCHAR(maxnumofchars), CHAR(1),
			DATE
- "INSERT INTO 'tablename' ('id','column1val',...,'columnNval');" to 
		insert a row of info in the current database
- "SELECT * FROM tablename;" to see the info in this table
- "DELETE from tablename where columnname=fieldtext;" to delete a row
		in this table where the column has this value
	
SOME USEFUL COMMAND LINE COMMANDS:
- ```vagrant up [--provider=docker]``` to create/start the container 
		corresponding to the Vagrantfile in the current directory [by using
		Docker as the provider and not VirtualBox if it does not detect it]
- ```vagrant docker-exec [VMname] -- command_to_execute``` to run a 
		one-off command against a Docker container by containername
		(error if container not running)
- ```vagrant docker-logs``` to see the logs of a running container
- ```vagrant reload [--provision]``` to reload container to include new 
		Vagrantfile commands [and reload provisions]
- ```vagrant destroy``` to shut down and deallocate resources corresponding 
		to container in this directory
- ```vagrant ssh``` to start ssh session into container in this directory 
		(end by typing "logout"); Uses Git, private key provided by Vagrant
		(Ubuntu VM supports ssh, but container does not)

EXECUTION OF VAGRANTFILE COMMANDS:
- "name" in "config.vm.define 'name' do |n|" is the same as the
		"config" variable.
- Commands placed inside the "config.vm.define 'name' do |n|" are
		applied only to the defined container (name).
- Commands placed outside this command are done to all containers.
- Commands are executed outside-in, in the order listed in the
		Vagrantfile.
