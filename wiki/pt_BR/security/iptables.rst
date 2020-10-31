********
Iptables
********

Iptables regras aplicadas na instalação

Regras Básicas
^^^^^^^^^^^^^^

::
     
  	iptablesF
	iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
	iptables -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT
	iptables -A INPUT -i lo -j ACCEPT
	iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
	iptables -A INPUT -p tcp --dport 22 -j ACCEPT
	iptables -P INPUT DROP
	iptables -P FORWARD DROP
	iptables -P OUTPUT ACCEPT
	iptables -A INPUT -p udp -m udp --dport 5060 -j ACCEPT
	iptables -A INPUT -p udp -m udp --dport 10000:20000 -j ACCEPT
	iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT

Regras Opcionais
^^^^^^^^^^^^^^^^

| OPENVPN: ``iptables -A INPUT -p udp --dport 1194 -j ACCEPT`` 
| ICMP: ``iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT``
| IAX: ``iptables -A INPUT -p udp -m udp --dport 4569 -j ACCEPT``
| HTTPS: ``iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT``

Scanner Amigável
^^^^^^^^^^^^^^^^^

Regras para bloquear scanner que não é amigável.

::
     
	iptables -I INPUT -j DROP -p tcp --dport 5060 -m string --string "friendly-scanner" --algo bm
	iptables -I INPUT -j DROP -p tcp --dport 5080 -m string --string "friendly-scanner" --algo bm
	iptables -I INPUT -j DROP -p udp --dport 5060 -m string --string "friendly-scanner" --algo bm
	iptables -I INPUT -j DROP -p udp --dport 5080 -m string --string "friendly-scanner" --algo bm

| *Opicional*


::
     
	iptables -I INPUT -j DROP -p tcp --dport 5060 -m string--string "VaxSIPUserAgent" --algo bm
	iptables -I INPUT -j DROP -p udp --dport 5060 -m string --string "VaxIPUserAgent" --algo bm
	iptables -I INPUT -j DROP -p udp --dport 5080 -m string --string "VaxSIPUserAgent" --algo bm
	iptables -I INPUT -j DROP -p tcp --dport 5080 -m string --string "VaxIPUserAgent" --algo bm


Mostrar regras iptables
^^^^^^^^^^^^^^^^^^^^^^^
::
     
  sudo iptables -L -v

Mostrar número de linha
^^^^^^^^^^^^^^^^^^^^^^^

::
     
  iptables -L -v --line-numbers

Deletar Linha
^^^^^^^^^^^^^^

Deletar linha 2

::
     
  iptables -D INPUT 2

Bloquear endereço de IP
^^^^^^^^^^^^^^^^^^^^^^

::
     
  iptables -I INPUT -s 62.210.245.132 -j DROP

Salvar mudanças
^^^^^^^^^^^^^

Centos
::
     
  service iptables save

Debian / Ubuntu

::
     
	apt-get install iptables-persistent
	service iptables-persistent save
	dpkg-reconfigure iptables-persistent



