# Configuração estática de Nomes

## Objetivo:
    * Configurar um serviço de nomes
    * configurar /etc/hosts

### Login da VM ubuntu server

* Usuário da VM: ``administrador``
* Senha da VM: ``adminifal``

## Serviço de nomes estático do Ubuntu:
>**_NOTA:_**
> Nomes de host estáticos são mapeamentos de nome de host para IP definidos localmente localizados no arquivo /etc/hosts. 
> Os nomes configurados no /etc/hosts têm precedência sobre o DNS por padrão. Assim, tentar resolver um nome de host e ele corresponder a uma entrada em /etc/hosts, ele não tentará procurar o registro no DNS. 
>O seguinte é um exemplo de um arquivo de hosts em que vários servidores locais foram identificados por nomes de host simples, aliases e seus nomes de >**_** domínio totalmente qualificados (FQDNs) equivalentes.

```
127.0.0.1 localhost
127.0.1.1 servidor ubuntu
10.0.0.11 server1 server1.example.com vpn
10.0.0.12 server2 server2.example.com mail
10.0.0.13 server3 server3.example.com www
10.0.0.14 server4 server4.example.com file
```

>**Observação**: No exemplo acima, observe que cada um dos servidores recebeu aliases além de seus nomes próprios e FQDNs. 
>* ``Server1`` foi mapeado para o nome ``vpn``
>* ``server2`` é referido como ``mail`` 
>* ``server3`` como ``www`` e 
>* ``server4`` como ``file``.

## Configurar o serviço de nomes estático.

```
Tabela 1: Definições de endereços IPs da Rede e Nomes de Hosts
-------------------------------------------------------------------------------------------------
|  DESCRICAO  |  IP             |   hostname        |          FQDN          |     aliase       |
-------------------------------------------------------------------------------------------------
| VM1-PC1     | 172.17.1.1      |   srv-vm1-pc1     | forousan.labredes.net  |       vpn        |
| VM2-PC1     | 172.17.1.3      |   srv-vm2-pc1     | stallings.labredes.net |       mail       |
| VM1-PC2     | 172.17.1.2      |   srv-vm1-pc2     | kurose.labredes.net    |       www        |
| VM2-PC2     | 172.17.1.4      |   srv-vm2-pc2     | tanembaum.labredes.net |       file       |
-------------------------------------------------------------------------------------------------
```

* Edite os arquivo /etc/hosts conforme as definições da Tabela de Endereços e Nomes (Tabela 1)

```shell
sudo nano /etc/hosts
```

* Exemplo do arquivo /etc/hosts na VM1:

```
127.0.0.1 localhost
127.0.1.1 srv-vm1-pc1   #Nome da VM 
172.17.1.1 srv-vm1-pc1 forousan.labredes.net vpn
172.17.1.2 srv-vm2-pc1 stallings.labredes.net mail
172.17.1.3 srv-vm1-pc2 kurose.labredes.net www
172.17.1.4 srv-vm2-pc2 tanembaum.labredes.net file

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

> **Fazer isso em todas as VMS!!!**

### Acessando uma VM remotamente:

* Exemplo: $ ssh ``<user>``@``<hostname|FQDN|alias|IP>``
* Fazendo o login 
   * de: terminal-pc
   * para: 192.168.56.101

```shell
ssh administrador@192.168.56.101
```

# Exercício:
Acessar uma VM a partir do terminal do PC e:

1) Ping para os hostnames, FQDNs e para os aliases que foram configurados nos 
2) Acessar uma VM a partir do terminal do PC e acesse as outras VMs utilizando os nomes.


