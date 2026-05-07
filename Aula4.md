# Roteiro SSH-Server (VM VirutalBox e Ubuntu-Server)

## Objetivo:
    * Configurar o acesso remoto via ssh nos servidores VM-LAB01 e VM-LAB02 dos PCs 1 e dois utilizados nas práticas anteriores.
    * Protocolo SSH, SCP, SFTP 
    * Porta 22
### Login nas VMs

* Usuário da VM: ``administrador``
* Senha da VM: ``adminifal``


```
Tabela 1: Definições de endereços IPs da Rede e Nomes de Hosts
-----------------------------------------------------
|  DESCRICAO  |  IP             |   hostname        |
-----------------------------------------------------
| rede        | 172.17.1.0      |                   |
| máscara     | 255.255.255.0   |                   |
| Gateway     | 172.17.1.1      |                   |
| VM1-PC1     | 172.17.1.1      |   srv-vm1-pc1     |
| VM2-PC1     | 172.17.1.3      |   srv-vm2-pc1     |
| VM1-PC2     | 172.17.1.2      |   srv-vm1-pc2     |
| VM2-PC2     | 172.17.1.4      |   srv-vm2-pc2     |
-----------------------------------------------------
```

### Atribuir nomes aos servidores ``hostname``

* formato: ``sudo hostnamectl set-hostname <hostname>``

* NA VM1-PC1 executar:
```shell
sudo hostnamectl set-hostname srv-vm1-pc1
```

* Fazer o mesmo nas outras VMs, seguindo as definições de nomes da tabela.


## Instalando o servidor SSH

### Antes de Começar:
   1. acessar as configurações de cada VM e altere novamente o Adaptador1 para **NAT**
   2. Comente as linhas de endereço IP estático e ative o DHCP nas configurações do Netplan
   
### Ceritifique-se que a VM está acessando a internet:

```shell
sudo apt update       # atualiza as definições e versões de pacotes/bibliotecas dos repositórios do ubuntu
sudo apt upgrade -y   # atualiza os pacotes com as novas definições e versões 
```

### Instale o SSH Server

```shell
systemctl status ssh
sudo apt-get install openssh-server
systemctl status ssh
```

### Verifique o status das portas do sistema
```
netstat -an | grep LISTEN.  #verifique as conexões TCP na porta 22 se está como LINSTENING
```

### Firewall 
* Para garantir o funcioamento correto do controle de acesso devemos configurar o firewall para permitir conexões remota via protocolo SSH, na porta 22.
 
```shell
sudo ufw status
sudo ufw allow ssh.    # ativa o ssh no firewall UFW do ubuntu.
sudo ufw status
```
    
* Para ativar o firewall:
```shell 
sudo ufw enable
```

### Refazendo a topologia de rede da Prática
* Retorne as configurações de interface de rede para o modo bridge em cada VM no VirtualBox e ative o endereçamento IP estático conforme a Tabela 1.

### Acessando uma VM remotamente:

* Exemplo: $ ssh ``<user>``@``<ipServidorRemoto>``
* Fazendo o login 
   * de: srv-vm1-pc1  
   * para: srv-vm2-pc1

```shell
ssh administrador@172.17.1.2
```



# Exercício:

1) Acessar a partir da VM1-PC1 todas as outras via ssh:
2) Crie dois usuários (use o comando ``sudo adduser``) em cada servidor com o nome dos alunos da dupla.
3) Faça o login via ssh nos servidores usando cada usuário criado.
