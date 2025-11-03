# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir = File.dirname(File.expand_path(__FILE__))
personal_config = YAML.load_file("#{current_dir}/config/personal.yml")
git_username = personal_config['git']['username']
git_password = personal_config['git']['password']
docker_url = "github.com/k-guti14/datatransfer.git"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", ip: "192.168.33.201"

  config.vm.synced_folder "./", "/vagrant", type: "virtualbox"
  config.vm.synced_folder "./workspace", "/home/vagrant/workspace", create: true, type: "virtualbox"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 3
    config.disksize.size = '40GB'
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.raw_arguments = ['-e allow_world_readable_tmpfiles=True', '-e pipelining=True']
    ansible.playbook = "provision/local.yml"
    ansible.extra_vars = {
      :git_username => git_username,
      :git_password => git_password,
      :docker_url   => docker_url
    }
  end

  ## データベースの作成
  #config.vm.provision "shell", inline: <<-SHELL
  #cd /var/www
  #SHELL

  end
