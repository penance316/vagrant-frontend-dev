# -*- mode: ruby -*-

dir = File.dirname(File.expand_path(__FILE__))

require 'yaml'
require "#{dir}/puphpet/ruby/deep_merge.rb"
require "#{dir}/puphpet/ruby/to_bool.rb"
require "#{dir}/puphpet/ruby/puppet.rb"

configValues = YAML.load_file("#{dir}/puphpet/config.yaml")

provider = ENV['VAGRANT_DEFAULT_PROVIDER'] ? ENV['VAGRANT_DEFAULT_PROVIDER'] : 'local'
if File.file?("#{dir}/puphpet/config-#{provider}.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-#{provider}.yaml")
  configValues.deep_merge!(custom)
end

if File.file?("#{dir}/puphpet/config-custom.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-custom.yaml")
  configValues.deep_merge!(custom)
end

data = configValues['vagrantfile']

Vagrant.require_version '>= 1.8.1'

Vagrant.configure('2') do |config|
  eval File.read("#{dir}/puphpet/vagrant/Vagrantfile-#{data['target']}")

############################################################
# Oh My ZSH Install section
config.vm.provision :shell, inline: "echo Installing zsh and oh-my-zsh"

# Install zsh prerequisites 
config.vm.provision :shell, inline: "apt-get -y install zsh"

# Clone Oh My Zsh from the git repo
config.vm.provision :shell, privileged: false,
inline: "if [ ! -d ~/.oh-my-zsh ] ; then
    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
fi" 

# Copy in the default .zshrc config file
config.vm.provision :shell, privileged: false,
inline: "cp -n ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc"

# Change the vagrant user's shell to use zsh
config.vm.provision :shell, inline: "sudo chsh -s /bin/zsh vagrant"

############################################################


end
