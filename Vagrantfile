# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :"dz9" => {
        :box_name => "generic/centos8"
      }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
    
          box.vm.provision :shell, :inline => "setenforce 0", run: "always"
          box.vm.provision "file", source: "script/", destination: "/tmp/"
          box.vm.provision "shell", inline: <<-SHELL
                        yum install epel-release -y
                        yum install spawn-fcgi php php-cli mod_fcgid httpd -y
                        sudo cp /tmp/script/watchlog.log /var/log/
			sudo cp /tmp/script/watchlog /etc/sysconfig
			sudo cp /tmp/script/watchlog.service /etc/systemd/system
			sudo cp -f /tmp/script/spawn-fcgi /etc/sysconfig/
			sudo cp /tmp/script/spawn-fcgi.service /etc/systemd/system
                        sudo cp /tmp/script/first.conf /etc/httpd/conf
			sudo cp /tmp/script/httpd-first /etc/sysconfig
                        sudo cp /tmp/script/httpd-second /etc/sysconfig
                        sudo cp '/tmp/script/httpd@first.service' /etc/systemd/system
			sudo cp '/tmp/script/httpd@second.service' /etc/systemd/system
                        sudo cp /tmp/script/first.conf /etc/httpd/conf
                        sudo cp /tmp/script/second.conf /etc/httpd/conf
			sudo cp /tmp/script/watchlog.sh /opt
			sudo cp /tmp/script/watchlog.timer /etc/systemd/system
                        chmod +x /opt/watchlog.sh
                        sudo systemctl start watchlog.timer
			sudo systemctl start watchlog.service
			sudo systemctl enable watchlog.timer
                        sudo systemctl start spawn-fcgi
                        sudo systemctl daemon-reload
			sudo systemctl start httpd@first
			sudo systemctl start httpd@second


          SHELL

      end
  end
end
