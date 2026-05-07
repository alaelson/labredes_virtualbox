# Configuração do OpenVPN 3 - Client no Linux

## Objetivo:
    * Configurar o cliente do Open VPN no linux para acessar o Laboratório Virtual de Redes (LabRedes)

### No terminal do PC com Linux do Laboratório Lab92

* faça login com o usuário ``redes``
```bash
su redes
cd ~
```


#### Pré-Requisitos para baixar o OpenVPN3

```bash
sudo apt install apt-transport-https -y
sudo apt install curl -y
sudo apt install gpg -y
```
#### Pré-Requisitos para baixar o OpenVPN3
```bash
curl -fsSL https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub | gpg --dearmor > ~/openvpn-repo-pkg-keyring.gpg
sudo mv openvpn-repo-pkg-keyring.gpg /etc/apt/trusted.gpg.d/openvpn-repo-pkg-keyring.gpg

curl -fsSL https://swupdate.openvpn.net/community/openvpn3/repos/openvpn3-focal.list > ~/openvpn3.list
sudo mv openvpn3.list /etc/apt/sources.list.d/openvpn3.list
sudo apt update
```

#### Instalar o OpenVPN3 

```bash
sudo apt install openvpn3
sudo apt install kmod-ovpn-dco
```

#### Importar o certificado para o OpenVPN 

* Baixe o arquivo com extensão ``.ovpn`` disponível no classroom
* Ou faça o download direto pelo terminal.

```bash
curl -fsSL https://www.dropbox.com/s/zlo0afhi96z6uua/vpn913.labredes.arapiraca.ifal.edu.br.ovpn?dl=0 > ~/vpn913.labredes.arapiraca.ifal.edu.br.ovpn
```


* ``CONFIG_FILE`` = arquivo de configuração .ovpn
* ``CONFIG_NAME``= nome da configuração

```bash
openvpn3 config-import --config CONFIG_FILE --name CONFIG_NAME --persistent
openvpn3 config-manage --show --config CONFIG_NAME --dco true
```
* Exemplo
```bash
openvpn3 config-import --config vpn913.labredes.arapiraca.ifal.edu.br.ovpn --name vpn913.labredes --persistent
openvpn3 config-manage --show --config vpn913.labredes --dco true
```

### Manipulando as configurações
 ```bash
 #lista as configurações importadas
openvpn3 configs-list
```
 ```bash
 #lista as configurações importadas
 openvpn3 config-remove --path CONFIG_PATH
```

### Manipulando a conexão VPN

#### Listar as conexões abertas
```bash
openvpn3 sessions-list
```

#### Iniciar a conexão
```bash
openvpn3 session-start --config CONFIG_NAME
```
* Exemplo
```bash
openvpn3 session-start --config vpn913.labredes
```

#### Finalizar a conexão
```bash
openvpn3 session-manage --config CONFIG_NAME --disconnect

openvpn3 session-manage --path CONFIG_PATH --disconnect
```
* Exemplo
```bash
openvpn3 session-manage --config vpn913.labredes --disconnect
```


### Acessando uma VM remotamente:

* Exemplo: $ ssh ``<user_vpn>``@``<ipServidorRemoto>``
* Fazendo o login 
   * de: terminal-pc
   * para: 10.9.13.100

```shell
ssh administrador@10.9.13.100
```


* Verifique a tabela de rotas e encontre o gateway da interface ``tun``(VPN Tunnel)

```shell
netstat -rn | grep -E "enp|tun"
```

* Verifique as conexões TCP/UDP (LISTEN e ESTABLISHED) e as portas abertas para o IP 

```shell
netstat -an | grep ESTABLISHED
```

## Outros comandos

### Comandos de log
```shell
openvpn3-admin version --services
openvpn3-admin log-service --log-level 6
```

```shell
journalctl --since -30m SYSLOG_IDENTIFIER=net.openvpn.v3.log + SYSLOG_IDENTIFIER=openvpn3-service-logger + SYSLOG_IDENTIFIER=dbus + _SYSTEMD_UNIT=dbus.service + UNIT=dbus.service
```
### NETSTAT: Conexões abertas na camada de transporte

```shell
netstat -an | grep LISTEN
```

### NETSTAT: com filtro para servidor de VPN
```shell
netstat -an | grep 200.133.132.60
```

### NETSTAT: tabela de rotas completa
```shell
netstat -rn 
```