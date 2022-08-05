# Step 1 :refresh your system’s package index
    $ sudo apt update
# Step 2 : Install ansible dependencies by running the following apt command
    $ sudo apt install software-properties-common
# Step 3 : Configure PPA repository for ansible
    $ sudo add-apt-repository --yes --update ppa:ansible/ansible
# Step 4 : Install latest version of Ansible
    $ sudo apt install ansible
# Step 5 : After the successful installation of Ansible, verify its version
    $ ansible --version