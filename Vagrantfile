$script = <<SCRIPT
(
apt install -y -q autoconf automake btrfs-progs docker gettext-devel git libcgroup-tools libtool python3-pip util-linux

fallocate -l 10G ~/btrfs.img
mkdir /var/bocker
mkfs.btrfs ~/btrfs.img
mount -o loop ~/btrfs.img /var/bocker

pip3 install git+https://github.com/larsks/undocker
systemctl start docker.service
docker pull centos
docker save centos | undocker -o base-image

ln -s /vagrant/bocker /usr/bin/bocker

echo 1 > /proc/sys/net/ipv4/ip_forward
iptables --flush
iptables -t nat -A POSTROUTING -o bridge0 -j MASQUERADE
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

ip link add bridge0 type bridge
ip addr add 10.0.0.1/24 dev bridge0
ip link set bridge0 up
) 2>&1
SCRIPT

Vagrant.configure(2) do |config|
	config.vm.box = 'ubuntu/bionic64'
	config.ssh.insert_key = 'true'
	config.vm.provision 'shell', inline: $script
end
