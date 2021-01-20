# bmc_discovery_webadmin
Shows how to install the latest version of `webadmin` tool on BMC Discovery appliance

## Webadmin
* http://www.webmin.com/
* https://sourceforge.net/projects/webadmin/
* https://github.com/webmin/webmin

## Installation
As the root user:
```bash
if [[ $(id -u) -eq 0 ]] ; then 
  FILE_WEBADMIN=webmin-current.rpm
  URL_WEBADMIN=http://www.webmin.com/download/rpm/${FILE_WEBADMIN}
  FILE_IPTABLES=/etc/sysconfig/iptables
  LINE=$(cat -n ${FILE_IPTABLES} | grep '\-A INPUT -i lo -j ACCEPT' | head -1 | awk '{print $1}')
  cp -avr ${FILE_IPTABLES} /etc/sysconfig/iptables.backup_$(date +'%d_%m_%Y_%H_%M_%S')
  sed -i "${LINE}i\# webadmin, port 10000\n-A INPUT -p tcp -m tcp --dport 10000 -j ACCEPT\n" ${FILE_IPTABLES}
  systemctl restart iptables
  wget -qP /tmp/ ${URL_WEBADMIN}
  rpm -i --nodeps /tmp/webmin-current.rpm
  rm -f /tmp/webmin-current.rpm
  echo -e "\033[1;92mGo to the http://$(hostname -I | awk '{print $1}'):10000 and login as a root\033[0m"
else
  echo -e '\033[1;91mYou are not root user. Switch to root and try again.\033[0m'
fi
```
