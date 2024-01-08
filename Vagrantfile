# ####################################################################
# ################### CONFIGURATION VARIABLES ########################
# ####################################################################

IMAGE_NAME = "bento/ubuntu-18.04"   # Image to use
MEM = 4096                          # Amount of RAM
CPU = 2                             # Number of processors (Minimum value of 2 otherwise it will not work)
MASTER_NAME="master"                # Master node name
WORKER_NBR = 4                      # Number of workers node
NODE_NETWORK_BASE = "192.168.56"    # First three octets of the IP address that will be assign to all type of nodes
POD_NETWORK = "10.244.0.0/16"       # Private network for inter-pod communication

# ######################## START VAGRANT #############################

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    # RAM and CPU config
    config.vm.provider "virtualbox" do |v|
        v.memory = MEM
        v.cpus = CPU
    end

    # Master node config
    config.vm.define MASTER_NAME do |master|
        
        # Hostname and network config
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "#{NODE_NETWORK_BASE}.10"
        master.vm.hostname = MASTER_NAME

        # Ansible role setting
        master.vm.provision "ansible" do |ansible|

            # Add debug mode
            #ansible.verbose = "vvv"

            ansible.compatibility_mode = "2.0"
            
            # Ansbile role that will be launched
            ansible.playbook = "roles/main.yml"

            # Groups in Ansible inventory
            ansible.groups = {
                "masters" => ["#{MASTER_NAME}"],
                "workers" => ["worker-[1:#{WORKER_NBR}]"]
            }

            # Overload Ansible variables
            ansible.extra_vars = {
                node_ip: "#{NODE_NETWORK_BASE}.10",
                node_name: "master",
                pod_network: "#{POD_NETWORK}"
            }
        end
    end

    # Worker node config
    (1..WORKER_NBR).each do |i|
        config.vm.define "worker-#{i}" do |worker|

            # Hostname and network config
            worker.vm.box = IMAGE_NAME
            worker.vm.network "private_network", ip: "#{NODE_NETWORK_BASE}.#{i + 10}"
            worker.vm.hostname = "worker-#{i}"

            # Ansible role setting
            worker.vm.provision "ansible" do |ansible|

                # Ansible role that will be launched
                ansible.playbook = "roles/main.yml"

                # Groups in Ansible inventory
                ansible.groups = {
                    "masters" => ["#{MASTER_NAME}"],
                    "workers" => ["worker-[1:#{WORKER_NBR}]"]
                }

                # Overload Ansible variables
                ansible.extra_vars = {
                    node_ip: "#{NODE_NETWORK_BASE}.#{i + 10}"
                }
            end
        end
    end
end