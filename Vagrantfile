VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :server do |server|
    server.vm.provider :aws do |aws, override|
      override.vm.box = 'dummy'

      aws.region                    = ENV['AWS_DEFAULT_REGION']
      aws.access_key_id             = ENV['AWS_ACCESS_KEY_ID']
      aws.secret_access_key         = ENV['AWS_SECRET_ACCESS_KEY']
      aws.keypair_name              = ENV['VAGRANT_AWS_KEY_PAIR']
      aws.instance_type             = 't2.small'
      aws.ami                       = 'ami-9d6fd8f3' # ubuntu 18.04 LTS
      aws.subnet_id                 = 'subnet-abcacae1' # Default subnet
      aws.security_groups           = [
        'sg-b8ad09d1', # Default SG
        'sg-5594bc3f', # Public ssh
      ]
      aws.associate_public_ip       = true
      override.ssh.username         = 'ubuntu'
      override.ssh.private_key_path = ENV['VAGRANT_SSH_KEY_PATH']
      override.ssh.pty              = true

      override.vm.synced_folder ".", "/vagrant", disabled: true
    end

    server.vm.provision "ansible" do |ansible|
      ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
      ansible.playbook = "provisioner/server.yml"
    end
  end
end

