=========== NANO KVM SEGURANÇA ===========

---- Ativar HTTPS ----
--->> Marcar em Network > HTTPS

---- Ativar SSH ----
--->> Marcar em Settings > Device > SSH

---- Editar porta WEB ----

nano /etc/kvm/server.yaml

http: 21995
https: 21993

Depois ir no painel web e reiniciar o KVM.

---- MUDAR PORTA SSH ----
nano /etc/ssh/sshd_config

descomentar
#Port 22
Port 2174

ctrl x para salvar e depois Y

Depois ir no painel web e reiniciar o KVM.


---- FIREWALL IPTABLES ----

1. Limpar regras antigas

iptables -F

2. Liberar localhost

iptables -A INPUT -i lo -j ACCEPT

3. Liberar conexões já estabelecidas (para não cair a sessão SSH)

iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

4. Liberar SSH apenas ao IP que desejar 
(mais de 1 ip faz 2x com o comando e novo ip)

iptables -A INPUT -p tcp -s IP_AQUI --dport 2174 -j ACCEPT

5. 7. Liberar NanoKVM HTTPS e HTTP somente para seus IPs 
(mais de 1 ip faz 2x com o comando e novo ip)

iptables -A INPUT -p tcp -s IP_AQUI --dport 21993 -j ACCEPT
iptables -A INPUT -p tcp -s IP_AQUI --dport 21995 -j ACCEPT

6. Ativar bloqueio padrão (bloquear tudo por padrão e só liberar o que precisar)

7. Verificar as regras
iptables -L -n -v

8. Salvar e conferir regras permanente **IMPORTANTE***

/etc/init.d/S35iptables save

cat /etc/iptables.conf


