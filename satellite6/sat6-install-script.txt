

#Register system to RHN


subscription-manager  register --username=martin.liu@redhat.com --password=XXXXXX

subscription-manager list --all --available


subscription-manager attach --pool=8a85f9814fcbf63f014fcfc66224794a
subscription-manager attach --pool=XXXXXX-Satellite-XXXXXX


subscription-manager repos --disable="*"


subscription-manager repos --enable rhel-7-server-rpms \
--enable rhel-server-rhscl-7-rpms \
--enable rhel-7-server-satellite-6.1-rpms


subscription-manager release --set=7.0Server

yum update -y



#Prepare OS system

/etc/hosts

192.168.16.3 sat6.xenlab.local



setenforce 0

yum install chrony -y
systemctl start chronyd
systemctl enable chronyd

firewall-cmd --add-port="53/udp" --add-port="53/tcp" \
 --add-port="67/udp" --add-port="68/udp" \
 --add-port="69/udp" --add-port="80/tcp" \
 --add-port="443/tcp" --add-port="5647/tcp" \
 --add-port="8140/tcp" \
&& firewall-cmd --permanent --add-port="53/udp" --add-port="53/tcp" \
 --add-port="67/udp" --add-port="68/udp" \
 --add-port="69/udp" --add-port="80/tcp" \
 --add-port="443/tcp" --add-port="5647/tcp" \
 --add-port="8140/tcp"

poweroff

take snapshoot for vm.

#Run installer

yum install katello

katello-installer -v \
--foreman-admin-username admin \
--foreman-admin-password smartvm \
--foreman-initial-organization "ACME" \
--foreman-initial-location "Beijing" \
--capsule-dns true \
--capsule-dns-interface eth0 \
--capsule-dns-zone xenlab.local \
--capsule-dns-forwarders 192.168.16.1 \
--capsule-dns-reverse 16.168.192.in-addr.arpa \
--capsule-dhcp true \
--capsule-dhcp-interface eth0 \
--capsule-dhcp-range "192.168.16.100 192.168.16.200" \
--capsule-dhcp-gateway 192.168.16.1 \
--capsule-dhcp-nameservers 192.168.16.3 \
--capsule-tftp true \
--capsule-tftp-servername $(hostname) \
--capsule-puppet true \
--capsule-puppetca true

poweroff

take snapshoot for vm.




