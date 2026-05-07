
* Abrir terminal
* logar com o usuário ``redes`` senha: admin@Lab92
```bash
 su redes
```

* Verifique se os diretórios abaixo existem. Se não, vide a [Aula 01](https://github.com/alaelson/labredes_virtualbox/blob/main/Aula1.md).

```
/labredes/images/original
/labredes/VM/BSI/<NomeDoAluno>
```


### Pelo Terminal 

```shell
# scp faz uma cópia de um arquivo em um computador remoto para um diretório em um computador local
# sintaxe: <user>@<server>:<path>/<file>
# user: aluno
# senha: aluno
# server: 192.168.101.10
# diretório do server: /Users/alaelson/Public
# diretório de destino: /labredes/images/original

cd /labredes/images/original
ls -la #verifique no resultado a existência dos arquivos .iso


```
* Instale o Virtualbox Extension Pack
```
su redes
sudo apt install virtualbox-ext-pack
```

## Criando uma Rede Ponto a Ponto com Duas Máquinas Virtuais

* Iremos criar uma rede ponto a ponto com duas VMs dentro do VirtualBox.
* Para isso tanto as VMs devem ser configuradas como as interfaces de rede dessas VMs.
* A Figura 1 ilustra a topologia de Rede dentro do VitualBox

<p><center> Figura 1: Topologia de Rede Ponto a Ponto usando o VitualBox, com duas VMs com suas NICs em modo Rede Interna</center></p>   
   <img src="figuresPTP/PTP-Network-Virtualbox.png" alt=""
    title="Figura 1: Topologia de Rede Ponto a Ponto" width="800" height="280" />

### Importar VMs no VirtualBox

* O arquivo .OVA é um formato de exportação de VM utilizado pelo VirtualBox
* Vamos importar este arquivo para criar as duas VMs que precisamos para fazer esta tarefa de rede ponto a ponto.

* A Figura 2 Ilustra as configurações para a importação das VMs: VM-LAB01 e VM-LAB02

<p><center> Figura 2: Criando uma VM apartir de um arquivo OVA</center></p>   
   <img src="figuresPTP/import-ova1.png" alt=""
    title="Figura 2a: Clique em Arquivo/Importar Apliance" width="400" height="280"/> <br/>
   <img src="figuresPTP/import-ova2.png" alt=""
    title="Figura 2b: configurações de importação" />

### Configurando as NICs das VMs
* Para que a VMs utilizem a mesma rede interna é necessário acessar as configurações de Rede de cada VM e selecionar o modo ``rede interna`` e definir o nome da rede, vamos escolher ``labredes`` como nome da nossa rede virtual. Utilize o mesmo nome nas duas VMs.


### Fazendo login nas VMs

* Usuário da VM: ``administrador``
* Senha da VM: ``adminifal``

<p><center> Figura 3: Telas das duas VMs em execução</center></p>   
   <img src="figuresPTP/dualVM.png" alt=""
    title="Figura 3: VMs em execução" width="1024" height="auto"/> <br/>


## Configuração estática de endereço IP na interface de rede 

* O Ubuntu utiliza um arquivo YAML para configurar as interfaces de rede
* este arquivo se contra na pasta ``/etc/netplan/``
* digite: 
```shell
ifconfig -a
ls -la /etc/netplan
cat /etc/netplan/01-netcfg.yaml
```
* Verifique o nome correto do arquivo no seu servidor. No exemplo a seguir, o nome do arquivo é ***01-netcfg.yaml***


### Na VM-Lab01

* instale as ferramentas de rede

```bash
$ sudo apt install net-tools -y
```
*  Edite o arquivo  ***01-netcfg.yaml*** 

```bash
$ sudo nano /etc/netplan/01-netcfg.yaml
```
*  Adicione as linhas para a configuração estática do IP para configurar o IP para ``172.17.0.2/24``. 
```
network:
    ethernets:
        enp0s3:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [172.17.0.1/24]    # IP e Máscara do Host.
            gateway4: 172.17.0.1          # IP do Gateway
            dhcp4: false                  # dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'
    version: 2
```
*  Após salvar o arquivo é necessário aplicar as configurações, com o **netplan apply**. Depois veja a configuração das interfaces com ****ifconfig -a***

```bash
$ sudo netplan apply
$ ifconfig -a
```

### Na VM-Lab02

* instale as ferramentas de rede

```bash
$ sudo apt install net-tools -y
```
*  Edite o arquivo  ***01-netcfg.yaml*** 

```bash
$ sudo nano /etc/netplan/01-netcfg.yaml
```
*  Adicione as linhas para a configuração estática do IP para configurar o IP para ``172.17.0.2/24``.

```
network:
    ethernets:
        enp0s3:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [172.17.0.2/24]    # IP e Máscara do Host.
            gateway4: 172.17.0.1          # IP do Gateway
            dhcp4: false                  # dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'
    version: 2
```
*  Após salvar o arquivo é necessário aplicar as configurações, com o **netplan apply**. Depois veja a configuração das interfaces com ****ifconfig -a***

```bash
$ sudo netplan apply
$ ifconfig -a
```
### Configuração da rede interna do VirtualBox

* A Figura 4 Ilustra as configurações para a importação das VMs: VM-LAB01 e VM-LAB02

<p><center> Figura 4: Configuração das NICs como modo ``rede interna``</center></p>   
   <img src="figuresPTP/VM-LAB01-Network.png" alt=""
    title="Figura 4a: VM-LAB01-Network" width="800" height="auto"/> <br/>
   <img src="figuresPTP/VM-LAB02-Network" alt=""
    title="Figura 4b: VM-LAB02-Network" width="800" height="auto"/>

### Teste a conectividade entre as VMs com o comando ``ping``

   * Ping da VM1 para VM2

```shell
ping 172.17.0.2       # ctrl + c para finalizar o comando
```
   * Ping da VM2 para VM1

```shell
ping 172.17.0.1       # ctrl + c para finalizar o comando
```
