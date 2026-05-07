# Configuração de teclado na VM

* **Locale** define as variáveis de ambiente que especificam a região, o idioma e as variantes de teclado do sistema que o usuário deseja ver na interface ou no terminal virtual.
As bibliotecas e as aplicações também verificam essas variável para carregaro idioma adequado de acordo com as preferências do usuário.

* A configurações de **Locale** basicamente consistem em um código de idioma (language code), um país/região (country/region code), data e formato de hora (time/date format), formatação numérica (numbers format setting), formatação monetária (currency format setting), cores (Color setting), etc. 


* Identifique os idiomas carregados no sistema com ``locale``

```shell
locale
```
  <img src="figuresLocale/locale-cmd.png" alt=""
	title="Figura 1: locale" width="800" height="auto" />
  <p><center> Figura 1: Saída de tela do comando **locale** antes da configuração.</center></p>   

*  O **localectl** é uma interface que facilita as configurações de locale no linux.
* ``localectl status`` exibe as configurações atuais de idioma e layout de teclado atuais.

```shell
localectl status
```
  <img src="figuresLocale/localectl-status-cmd.png" alt=""
	title="Figura 2: locale" width="800" height="auto" />
  <p><center> Figura 2: Saída de tela do comando **locale status** antes da configuração.</center></p>   

* Instale o pacote de idiomas para português:

```shell
sudo apt install language-pack-pt -y
```

* Verifique se o pacote de idioma foi instalado

```shell
locale -a
```

## Configuração do idioma e teclado para ``pt_BR`` 
* Escolha o pacote pt_BR.utf8
```shell
sudo localectl set-locale LANG=pt_BR.utf8
sudo localectl set-x11-keymap br abnt2 
localect status # verifique a saída de tela 
```

<img src="figuresLocale/localectl-setlocale-cmd.png" alt=""
	title="Figura 3: configuração do idioma e do layout de teclado para pt-br" width="800" height="auto" />
  <p><center> Figura 3: Configuração do idioma e do layout de teclado para pt-br.</center></p>   

* Reinicie a VM com ``sudo reboot``
