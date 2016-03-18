VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.customize [ 'modifyvm', :id, '--memory', '512' ]
    vb.customize [ 'modifyvm', :id, '--nictype1', 'virtio' ]
    vb.customize [ 'modifyvm', :id, '--natdnshostresolver1', 'on' ]
    vb.customize [ 'modifyvm', :id, '--natdnsproxy1', 'on' ]
  end
      
  config.vm.define 'ubuntu' do |ubuntu|
    ubuntu.vm.box      = 'ubuntu/trusty64'
    ubuntu.vm.hostname = 'ubuntu'
    
    ubuntu.vm.provision 'shell', inline: 'apt-get update'
    ubuntu.vm.provision 'shell', inline: 'apt-get install -y -qq  python-pip'
    ubuntu.vm.provision 'shell', inline: 'pip install ansible jinja2'

    ubuntu.vm.provision 'ansible' do |ansible| 
      ansible.playbook = 'tests/test_vagrant.yml'
      ansible.extra_vars = {
        filebeat_user:  'root',
        filebeat_group: 'root',
        filebeat_create_user: false,
      }
    end
    
  end

  config.vm.define 'centos-6' do |centos|
    EPEL_REPO = '''
[epel]
name     = EPEL 6 - \$basearch
baseurl  = http://mirror.globo.com/epel/6/\$basearch
enabled  = 1
gpgcheck = 0
'''

    centos.vm.box      = "puppetlabs/centos-6.6-64-nocm"
    centos.vm.hostname = 'centos'

    centos.vm.provision 'shell', inline: 'yum install -y ca-certificates'
    centos.vm.provision 'shell', inline: "echo \"#{EPEL_REPO}\" > /etc/yum.repos.d/epel.repo"
    centos.vm.provision 'shell', inline: 'yum install -y python-pip python-devel gcc'
    centos.vm.provision 'shell', inline: 'pip install -q pip --upgrade'
    centos.vm.provision 'shell', inline: 'pip install -q ansible jinja2'

    centos.vm.provision 'ansible' do |ansible| 
      ansible.playbook   = 'tests/test_vagrant.yml'
      ansible.extra_vars = {
        filebeat_user:  'root',
        filebeat_group: 'root',
        filebeat_create_user: false,
      }
    end
  end

  config.vm.define 'centos-7' do |centos|
    EPEL_REPO = '''
[epel]
name     = EPEL 7 - \$basearch
baseurl  = http://mirror.globo.com/epel/7/\$basearch
enabled  = 1
gpgcheck = 0
'''

    centos.vm.box      = 'centos/7'
    centos.vm.hostname = 'centos'

    centos.vm.provision 'shell', inline: 'yum install -y ca-certificates'
    centos.vm.provision 'shell', inline: "echo \"#{EPEL_REPO}\" > /etc/yum.repos.d/epel.repo"
    centos.vm.provision 'shell', inline: 'yum install -y python-pip python-devel gcc'
    centos.vm.provision 'shell', inline: 'pip install -q pip --upgrade'
    centos.vm.provision 'shell', inline: 'pip install -q ansible jinja2'

    centos.vm.provision 'ansible' do |ansible| 
      ansible.playbook = 'tests/test_vagrant.yml'
      ansible.extra_vars = {
        filebeat_user:  'root',
        filebeat_group: 'root',
        filebeat_create_user: false,
      }
    end
  end

end
  
# vi:ts=2:sw=2:et:ft=ruby:
