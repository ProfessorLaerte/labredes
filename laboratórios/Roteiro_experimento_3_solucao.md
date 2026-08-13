**rede original**

```
192.168.0.0/24
```


| **Nome da Rede** | **Endereço da Rede** | **Broadcast** | **Máscara CIDR** | **Hosts Utilizáveis** | **Primeiro Host** | **Último Host** |
| ---------------- | -------------------- | ------------- | ---------------- | --------------------- | ----------------- | --------------- |
| Administrativo   | 192.168.0.0          | 192.168.0.127 | /25              | 126                   | 192.168.0.1       | 192.168.0.126   |
| Vendas           | 192.168.0.128        | 192.168.0.191 | /26              | 62                    | 192.168.0.129     | 192.168.0.190   |
| TI               | 192.168.0.192        | 192.168.0.223 | /27              | 30                    | 192.168.0.193     | 192.168.0.222   |
| Convidados       | 192.168.0.224        | 192.168.0.255 | /27              | 30                    | 192.168.0.225     | 192.168.0.254   |



## R1 - configuração

```
enable 
conf t
hostname R1
int fa0/0
ip add 192.168.0.129 255.255.255.192
no shut
int fa0/1
ip add 192.168.0.193 255.255.255.224
no shut
int eth0/1/0
ip add 192.168.0.225 255.255.255.224
no shut
exit

no ip domain-lookup
enable secret labredes
security passwords min-length 10
service password-encryption

banner motd #
ATENÇÃO: Acesso restrito!
Somente usuários autorizados podem entrar.
Tentativas não autorizadas serao registradas.#

ip domain-name labredes.com
username aluno secret 1234567890

crypto key generate rsa
1024

line vty 0 15
transport input ssh
login local
exec-timeout 6
exit
login block-for 180 attempts 4 within 120
```



## Sw1 - Configuração

```
enable 
conf t
hostname Sw1
int vlan1
ip add 192.168.0.190 255.255.255.192
no shut
exit
ip default-gateway 192.168.0.129
no ip domain-lookup
enable secret labredes
service password-encryption
banner motd #
ATENÇÃO: Acesso restrito!
Somente usuários autorizados podem entrar.
Tentativas não autorizadas serao registradas.#
ip domain-name labredes.com
username aluno secret 1234
crypto key generate rsa
1024
line vty 0 15
transport input ssh
login local
exec-timeout 6
exit
```



## Sw2 - Configuração

```
enable 
conf t
hostname Sw2
int vlan1
ip add 192.168.0.222 255.255.255.192
no shut
exit

ip default-gateway 192.168.0.193
no ip domain-lookup
enable secret labredes
service password-encryption

banner motd #
ATENÇÃO: Acesso restrito!
Somente usuários autorizados podem entrar.
Tentativas não autorizadas serao registradas.#

ip domain-name labredes.com
username aluno secret 1234

crypto key generate rsa
1024

line vty 0 15
transport input ssh
login local
exec-timeout 6
exit
```



## Sw3 - Configuração

```
enable 
conf t
hostname Sw3
int vlan1
ip add 192.168.0.254 255.255.255.192
no shut
exit

ip default-gateway 192.168.0.225
no ip domain-lookup
enable secret labredes
service password-encryption

banner motd #
ATENÇÃO: Acesso restrito!
Somente usuários autorizados podem entrar.
Tentativas não autorizadas serao registradas.#

ip domain-name labredes.com
username aluno secret 1234

crypto key generate rsa
1024

line vty 0 15
transport input ssh
login local
exec-timeout 6
exit
```





