
#!/usr/bin/env bash

yum -y install ntp 
mkdir -p /data1/primary /data2/primary /data1/mirror /data2/mirror
#使用之前修改网络地址,和对于的网口
#sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
iptables -F
service iptables save
service atd                         stop
service auditd                      stop
service cups                        stop
service ip6tables                   stop
service iptables                    stop
service postfix                     stop
service NetworkManager              stop
service acpidavahi-daemon           stop
service bluetooth                   stop
service cpuspeed                    stop                     
chkconfig      atd                   off                                
chkconfig      auditd                off
chkconfig      cups                  off
chkconfig      ip6tables             off
chkconfig      iptables              off
chkconfig      postfix               off
chkconfig      NetworkManager        off
chkconfig      acpidavahi-daemon     off
chkconfig      bluetooth             off
chkconfig      cpuspeed              off
chkconfig      ntpd									 on
#sed -i -e 's/ONBOOT=no/ONBOOT=yes/g' -e 's/NM_CONTROLLED=yes/NM_CONTROLLED=no/g' -e 's/BOOTPROTO=dhcp/BOOTPROTO=static/g' /etc/sysconfig/network-scripts/ifcfg-eth0
#sed -i -e 's/ONBOOT=no/ONBOOT=yes/g' -e 's/NM_CONTROLLED=yes/NM_CONTROLLED=no/g' -e 's/BOOTPROTO=dhcp/BOOTPROTO=static/g' /etc/sysconfig/network-scripts/ifcfg-eth
#cat >>/etc/sysconfig/network-scripts/ifcfg-bond4<<EOF
#BONDING_OPTS="miimon=100 mode=4 xmit_hash_policy=1"
#BOOTPROTO="static"
#DEVICE="bond4"
#IPV6INIT="no"
#NM_CONTROLLED="no"
#ONBOOT="yes"
#IPADDR="10.249.79.12"
#NETMASK="255.255.255.0"
#EOF
#配置NTP 服务
echo "server mdw" >> /etc/ntp.conf
sed -i 's/server 0.centos.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf
sed -i 's/server 1.centos.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf
sed -i 's/server 2.centos.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf


sed -i 's/server 0.rhel.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf
sed -i 's/server 1.rhel.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf
sed -i 's/server 2.rhel.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf
sed -i 's/server 3.rhel.pool.ntp.org/#server 0.centos.pool.ntp.org/g' /etc/ntp.conf


/etc/init.d/ntpd start

cat >>/etc/sysctl.conf <<EOF
#RHEL 4.3.3
kernel.shmmax = 500000000
kernel.shmmni = 4096
kernel.shmall = 4000000000
kernel.sem = 250 512000 100 2048
kernel.sysrq = 1
kernel.core_uses_pid = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.msgmni = 2048
net.ipv4.tcp_syncookies = 1
net.ipv4.ip_forward = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_max_syn_backlog = 4096
net.ipv4.conf.all.arp_filter = 1
net.ipv4.ip_local_port_range = 1025 65535
net.core.netdev_max_backlog = 10000
net.core.rmem_max = 2097152
net.core.wmem_max = 2097152
vm.overcommit_memory = 2
kernel.core_pattern=/var/crash/user/core.%e.%p.%t.%s.%u.%g
EOF

cat >>/etc/security/limits.conf<<EOF
* soft nofile 131072
* hard  nofile 131072
* soft  core 4194304
# these nproc values are overridden in limits.d/90-nproc.conf
* soft  nproc 131072
* hard  nproc 131072
EOF
cat >>/etc/security/limits.d/90-nproc.conf <<EOF
* soft nproc 131072
* hard nproc 131072
EOF
echo 'blockdev --setra 16384 /dev/sd*' >>/etc/rc.local 

cat >>/etc/rc.local <<!
echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
!
cat >>/etc/fstab<<EOF
/dev/sdb1 /data1 xfs nodev,noatime,inode64,allocsize=16m 0 0
/dev/sdc1 /data2 xfs nodev,noatime,inode64,allocsize=16m 0 0
EOF
