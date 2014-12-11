1. vagrant global-status -> to get docker host VM id
ce33d79  default     virtualbox running   /Users/mwang/.vagrant.d/data/docker-host

2. vagrant ssh ce33d79 -> to getin docker Host 

3. ssh -i insecure_key root@<IP>

➜  test $ vssh ce33d79
                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
boot2docker: 1.2.0
             3.16.1-config-file : e75396e - Fri Aug 22 06:45:30 UTC 2014
docker@boot2docker:~$ ssh -i insecure_key root@172.17.0.3
Last login: Wed Dec 10 10:46:26 2014 from 172.17.42.1
root@gru:~#

4. Vagrantfile

ENV['VAGRANT_DEFAULT_PROVIDER'] ||= 'docker'

Vagrant.configure("2") do |config|

  config.vm.provider "docker" do |d|

    d.image = "phusion/baseimage:0.9.15"
    #d.ports = ["22:22"]
    d.has_ssh = true

    # Is this necessary if EXPOSE is used in Dockerfile?
    d.expose = ["22"]
    d.remains_running = true
    d.volumes = ["/shared"]
    d.cmd = ["/sbin/my_init", "--enable-insecure-key"]

  end

  config.vm.network :private_network, ip: "10.0.1.129"
  config.vm.hostname = "gru"
  #config.vm.network "forwarded_port", guest: 2222, host: 22
  config.vm.synced_folder "~/Documents/shared", "/shared"

  #config.vm.provision "chef_solo" do |chef|
  #  chef.add_recipe "apache"
  #end
end

5. Run

vagrant up --provider=docker

6. On Mac OS X

will hang on     default: Container created: 7f06f900d6b34d5c
==> default: Starting container...
==> default: Waiting for machine to boot. This may take a few minutes...

you can use CTRL-C to break the connection, but the VM is running already.

PS. will add vagrant account to resolve this problem.

