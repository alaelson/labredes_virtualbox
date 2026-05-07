
* Abrir terminal
* logar com o usuário ``redes`` senha: ``admin@Lab92``
```bash
 su redes
```

* Alguns comandos básicos:
```shell
 cd ~
 pwd
 /home/redes
```
* listar arquivo e pastas
```shell
 ls -la #lista todos os arquivos e pastas
total 20
drwxr-xr-x 2 redes redes 4096 Jun 15 17:48 .
drwxr-xr-x 5 root  root  4096 Jun 15 17:50 ..
-rw-r--r-- 1 redes redes  220 Jun 15 17:48 .bash_logout
-rw-r--r-- 1 redes redes 3771 Jun 15 17:48 .bashrc
-rw-r--r-- 1 redes redes  807 Jun 15 17:48 .profile
```
* manipulando um arquivo
```shell
 touch meuarquivo.txt #cria um arquivo com o nome meuarquivo.tst
 ls -la
total 20
drwxr-xr-x 2 redes redes 4096 Jun 17 12:10 .
drwxr-xr-x 5 root  root  4096 Jun 15 17:50 ..
-rw-r--r-- 1 redes redes  220 Jun 15 17:48 .bash_logout
-rw-r--r-- 1 redes redes 3771 Jun 15 17:48 .bashrc
-rw-rw-r-- 1 redes redes    0 Jun 17 12:10 meuarquivo.txt #  --> arquivo criado
-rw-r--r-- 1 redes redes  807 Jun 15 17:48 .profile
 cd ..
 pwd
/home
 cd redes/
 pwd 
/home/redes
 cat meuarquivo.txt 
 nano meuarquivo.txt # editar texto com o nano
 cat meuarquivo.txt 
Meu primeiro arquivo editável no linux.
 ls -la
total 28
drwxr-xr-x 3 redes redes 4096 Jun 17 12:18 .
drwxr-xr-x 5 root  root  4096 Jun 15 17:50 ..
-rw-r--r-- 1 redes redes  220 Jun 15 17:48 .bash_logout
-rw-r--r-- 1 redes redes 3771 Jun 15 17:48 .bashrc
drwxrwxr-x 3 redes redes 4096 Jun 17 12:16 .local
-rw-rw-r-- 1 redes redes   41 Jun 17 12:18 meuarquivo.txt
-rw-r--r-- 1 redes redes  807 Jun 15 17:48 .profile``
 vi meuarquivo.txt # editar texto com o vi
 cat meuarquivo.txt 
Meu primeiro arquivo editável no linux.
Editado pelo vi.
```

* criar pasta ``labredes`` na raiz ``/`` e subpastas
```bash
 sudo mkdir /labredes
 cd /
 ls -la #verifique se a pasta `labredes` foi criada
```
```bash
 ls -la / #verificar a existencia do diretório /labredes
 cd /labredes 
 ls -la / #verificar a existencia do diretório /labredes/imagens
 sudo mkdir images
 cd images
 sudo mkdir original 
 cd original
 ls -la

# cria diretórios e subdiretórios
 cd /
 sudo mkdir labredes/VM
 sudo mkdir labredes/VM/BSI
 sudo mkdir labredes/VM/BSI/<student> # substitua <student> pelo seu nome
```


* adiciona o usuario ``aluno`` ao grupo ``redes``
```bash
 sudo usermod -aG redes aluno
```


* Modificando as permissões de arquivos e pastas
   * ``chown`` modifica o dono da pasta labredes para o usuario nobody e grupo nogroup
   * ``chgrp`` altera o proprietário de grupo do diretório ``/labredes`` para o grupo ``redes``
   * ``chmod`` altera as permissões do diretório para escrita pelos membros do grupo

```bash
 sudo chown -R nobody:nogroup /labredes
 ls -la
 sudo chgrp -R redes /labredes
 sudo chmod -R 771 /labredes 
 ls -la
 getent group  #lista grupos: observe no fim da lista que os usuários também possuem grupos
```


* Verifique se os diretórios existem:
```
/labredes/images/original
/labredes/VM/BSI/<NomeDoAluno>
```
* Verifique se os arquivos existem no diretório /labredes/images/original
```
mini.iso
ubuntu-20.04.4-desktop-amd64.iso
ubuntu-22.04-live-server-amd64.iso
```
### Pelo Nautilus (Arquivos), acessar:

	smb://192.168.101.10/iso-images
	- user: aluno, senha: aluno

	* copiar os arquivos da pasta public para a pasta  ``/labredes/images/original``

### Pelo Terminal 

```shell
# scp faz uma cópia de um arquivo em um computador remoto para um diretório em um computador local
# sintaxe: <user>@<server>:<path>/<file>
# user: aluno
# senha: aluno
# server: 192.168.101.10
# diretório do server: ~/Public/iso-images/
# diretório de destino: /labredes/images/original

cd /labredes/images/original
ls -la #verifique no resultado a existência dos arquivos .iso

# Se não houver os arquivos iso na pasta /labredes/images/original deve-se copiá-los com os comandos:
scp aluno@192.168.101.10:~/Public/iso-images/mini.iso /labredes/images/original
scp aluno@192.168.101.10:~/Public/iso-images/ubuntu-20.04.4-desktop-amd64.iso /labredes/images/original
scp aluno@192.168.101.10:~/Public/iso-images/ubuntu-22.04-live-server-amd64.iso /labredes/images/original

```

### No Windows Explorer
```
# sintaxe: \\<server>\<path>\<file>
# user: aluno
# senha: aluno
# server: 192.168.101.10
# diretório do server: /Users/Shared/iso-images
# diretório de destino: /labredes/images/original
# abra o Windows Explorer digite o comando na barra de endereços: \\192.168.101.10\iso-images
```




