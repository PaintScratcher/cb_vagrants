  # Number of nodes to provision
  num_nodes = 1

  # Define Number of RAM for each node
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
  end

  if (defined?(Url)).nil?
    Url= "http://packages.couchbase.com/releases/#{Version}/couchbase-server-enterprise_#{Version}_x86_64"
  end

  # Provision the server itself with puppet
  config.vm.provision "puppet" do |puppet|
    puppet.manifests_path = "../../"
    puppet.manifest_file = "puppet.pp"
    puppet.facter = {
      "version" => Version,
      "url" => Url,
    }
  end

  # Provision Config for each of the nodes
  1.upto(num_nodes) do |num|
    config.vm.define "node#{num}" do |node|
      node.vm.box = box
      node.vm.network :private_network, :ip => ip_addr_base % num
      node.vm.provider "virtualbox" do |v|
        v.name = "Couchbase Server #{Version} #{box} Node #{num}"
      end
    end
  end

  if ARGV[0] == "up"
    puts "\e[32m=== Created #{num_nodes} node(s) on #{box} and cb version #{Version} ==="
  end
