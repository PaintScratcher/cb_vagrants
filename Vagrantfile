# Number of nodes to provision
unless ENV['VAGRANT_NODES'].nil? || ENV['VAGRANT_NODES'] == 0
  num_nodes = ENV['VAGRANT_NODES'].to_i
else
  num_nodes = 4
end

# Check to see if a custom download location has been given, if not use a default value (2.5.0 style)
if (defined?(Url)).nil?
  Url= "http://packages.couchbase.com/releases/#{Version}/couchbase-server-enterprise_#{Version}_x86_64"
end

# Start the vagrant configuration
Vagrant.configure("2") do |config|

  # Define Number of RAM for each node
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
  end

  if defined?(box_url)
    # Define the vagrant box download location from the link given in the bottom level vagrantfile
    config.vm.box_url = box_url
  end

  # Check to see if the VM is not running Windows and provision with puppet
  if !(box.include?("win"))
    # Provision the server itself with puppet
    config.vm.provision "puppet" do |puppet|
      puppet.manifests_path = "../../" # Define a custom location and name for the puppet file
      puppet.manifest_file = "puppet.pp"
      puppet.facter = { # Pass variables to puppet
        "version" => Version, # Couchbase Server version
        "url" => Url, # Couchbase download location
      }
    end
  end

  # Provision Config for each of the nodes
  1.upto(num_nodes) do |num|
    config.vm.define "node#{num}" do |node|
      node.vm.box = box
      node.vm.network :private_network, :ip => ip_addr_base % num
      node.vm.provider "virtualbox" do |v|
        v.name = "Couchbase Server #{Version} #{box.gsub '/', '_'} Node #{num}"
        if(box.include?("win")) # If the VM is running Windows it will start with a GUI
          v.gui = true
        end
      end
    end
  end

  # Check to see if the vagrant command given was 'up', if so print a handy dialogue
  if ARGV[0] == "up"
    puts "\e[32m=== Created #{num_nodes} node(s) on #{box} and cb version #{Version} ==="
  end
end
