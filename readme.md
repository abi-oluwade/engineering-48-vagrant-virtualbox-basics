
# Vagrant and VirtualBox
Vagrant will isolate dependencies and their configuration within a single disposable, consistent environment, without sacrificing any of the tools you are used to working with (editors, browsers, debuggers, etc.). Once you or someone else creates a single ‘Vagrantfile’, you just need to vagrant up and everything is installed and configured for you to work. So, for instance you can create multiple vagrant files in 3 separate folders and navigate to each folder, use the command ‘vagrant u’' and the virtual machine will start up with that specific configuration of the vagrant file and load the provisions script specified in the ‘Vagrantfile’ path for example.
Other members of your team create their development environments from the same configuration, so whether you are working on Linux, Mac OS X, or Windows, all your team members are running code in the same environment, against the same dependencies, all configured the same way.
Vagrant gives you a disposable environment and consistent workflow for developing and testing infrastructure management scripts. You can quickly test things like shell scripts, Chef cookbooks, Puppet modules, and more using local virtualization such as VirtualBox or VMware. Then, with the same configuration, you can test these scripts on remote clouds such as AWS or RackSpace with the same workflow.

## vagrant init
This initializes the vagrant environment, allowing us to access the vagrant file and thus specify the config which we want the virtual environment to use. EXTERMELY IMPORTANT POINT, for each config , you must navigate to the folder with that vagrantfile configurations you want and use 'vagrant up'
in that folder to spin up a virtual machine with those specific configs. Imagine 3 separate cds with different builds on them, being put into a virtual machine, but rather than physical cds we have folders which hold the vagrantfile and rather than physically inserted, are spun up using 'vagarnt up'

## Spinning up a machine/OS
The Vagrantfile config can be specified to load ubuntu/xenial64 OS then be spun up by using ‘vagrant up’ which will start up the virtual machine OS.

## ssh
Vagrant ssh allows the host machine to make a secure connection to the virtual machine which means we can use the git bash terminal to give commands via the command line to the virtual machine os.

## provision.sh
First, every provision script is configured within your Vagrantfile using the config.vm.provision method call. For example, the Vagrantfile below enables shell provisioning:
````
Vagrant.configure("2") do |config|
  # ... other configuration

  config.vm.provision "shell", inline: "echo hello"
end
````
Every provision script has a type, such as "shell", used as the first parameter to the provisioning configuration. Following that is basic key/value for configuring that specific provision script.
The provision script which will have its path specified in the Vagrantfile can automate a lot of things for us such as the installing of scripts/dependencies, libraries and executing commands. -y means the terminal will auto select yes for any confirmations needed.

## npm install and npm start
This command installs a package, and any packages that it depends on. If the package has a package-lock or shrinkwrap file, the installation of dependencies will be driven by that, with an npm shrinkwrap.json taking precedence if both files exist. This runs an arbitrary command specified in the package’s "start" property of its "scripts" object. If no "start" property is specified on the 'scripts' object, it will run node server.js. ( in our ‘app’ package , the start property is assigned to the local host3000, which allows us to go the website specified.)

## synced_folder
Synced folders must be specified in the vagrantfile so that the virtual machine can navigate to it. They have the following syntax:
````
#creates connection for the synced folders betwwen host and vm
config.vm.synced_folder "environment/db", "/home/ubuntu/environment"
````


### Task

The main task was to execute the virtual machine and access the app-website on the port 3000. We achieved this by first:
- setting up the vagrant file appropriately by assigning the synced folder on the host machine to the synced folder on the virtual machine,
so that our virtual machine now has access to in this case the 'app' folder and can be accessed via the '/app' path in the vm.

- config.vm.provison tells the virtual machine the location of the bash script that will auto install dependencies and auto execute commands, by telling
it which path the provision script is in.


IPV4 : 192.168.56.1
ipv4 Network Mask: 255.255.255.0


### Setting up the SSH (weird work around)
- Take note of the ipv4 ip, the network mask and ensure dhcp is unchecked.
and ensure we have a host only network available.

- next for the VM network preferences ensure adapter 1 and 2 are enabled with
adapter 2 selecting the host-only network settings. With adapter 1 being set
to NAT(which allows our guest to connect to the internet though our host)

- sudo nano /etc/hostname = change name of VM
sudo nano /etc/hosts = lets us change the host name too(?)
ifconfig -a = lets us check the inet address and the name of the second interface
e.g enp0s8(second adapter name)
sudo nano /etc/network/interfaces = auto (enp0s8) /iface (enp0s8) inet static/
address <set the ip> *dashes mean new line*/ netmask<set the subnet mask>

- 'ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@192.168.33.71
'this command with the ip address you have set will connect the host and server
SSH.

https://www.youtube.com/watch?v=S7jD6nnYJy0&t= <----- follow if lost
sudo apt-get update && sudo apt-get dist-upgrade (installs packages)

-y lets the shell automatically accept 'yes' as an answer to the installation.

- To install multiple packages each time, create a provision.sh script, give it
the 'sudo apt-get <whatever>', put it in the 'environment' , config the
vagrant-file appropriately and run vagrant up and vagrant ssh. And set up
the provison.sh.

- when you start up vagrant ssh, it lets you use the terminal to accsess
the machine, however you are still just in the base home menu
i.e (ubuntu/xenial64~) so you still have to navigate to the location of
the folder that you want to access. If vagrant init is not used, you CANNOT
go to the folder.
