# Debian GNU/Linux Vim and ZSH setup

## Intro

En esta guia se describe la configuración del sistema operativo GNU/Linux para adaptarlo a un ambiente de trabajo para
realizar tareas relacionadas a la administración de:

* Linux personal
* Servidores Linux
* Recursos cloud
* Clusters Kubernetes
* Desarrollo de software

Se utilizará Software Libre, es decir, software GNU, el kernel Linux y en especifico la distro Debian GNU/Linux 11
Bullseye en una arquitectura x86 de 64-bit.

Para esto necesitamos dejar algunas cosas bien configuradas:

* Entornos de desarrollo
* Gestores de paquetes
* Editor de textos
* Emulador de terminal
* Entorno del shell
* Cliente VPN
* Navegador web

## Objetivos

Realizaremos las siguientes actividades orientadas a instalar y configurar:

* Herramientas de linea de comando GNU
* Gestor de paquetes Apt
* Editor de textos vim
* Emulador de terminal Terminator
* El shell zsh con oh-my-zsh
* Herramientas de búsqueda en linea de comandos
* Conjunto de herramientas de linea de comandos
* Herramientas de gestión Cloud

## Gestor de paquetes APT

El programa `apt` es la herramienta de gestión de paquetes en `Debian`.

Apt ya viene pre instalado en Debian 11 Bullseye por lo que no hay que instalarlo por separado.

Lo primero que debemos aprender a usar es actualizar la lista de paquetes de los repositorios:

```shell
$ sudo apt update
```

Ahora actualizamos los paquetes del sistema:

```shell
$ sudo apt upgrade
```

**NOTA:** Estas dos tareas las debemos de ejecutar frecuentemente para mantener nuestro sistema actualizado.

Buscando paquetes:

```shell
$ sudo apt search git
```

```shell
$ sudo apt install git
```

Desinstalando paquetes:

```shell
$ sudo apt remove git
```

Actualizando paquetes:

```shell
$ sudo apt update; sudo apt upgrade
```

Limpieza de paquetes viejos:

```shell
$ sudo apt clean; sudo apt autoremove
```

## Instalando comandos GNU

Instalaremos los programas `wget` y `curl` los cuales usaremos para descargar cosas desde
internet usando la línea de comandos:

```shell
$ sudo apt install wget curl
```

## Editor de textos vim

Instalación de vim:

```shell
$ sudo apt install vim
```

Editar config:

```shell
$ vim ~/.vimrc
```

Agregamos el contenido inicial:

```shell
" .vimrc

" Configuración general
set title " Muestra el nombre del archivo en la ventana de la terminal
set number " Muestra los números de las líneas
set nowrap " No dividir la línea si es muy larga
set cursorline " Resalta la línea actual
set colorcolumn=120 " Muestra la columna límite a 120 caracteres
set nocompatible " Desactiva modo compatible
filetype plugin on " Habilita plugin para tipos de archivos
syntax on " Activa resaltado de sintaxis

" Indentación a 2 espacios
set tabstop=2
set shiftwidth=2
set softtabstop=2
set shiftround
set expandtab " Insertar espacios en lugar de <Tab>s
set imrmguicolors " Activa true colors en la terminal

" Configuracion spell check
"set spell
set nospell
setlocal spell spelllang=es,en " Corregir palabras usando diccionarios en español

" Mapeos
"inoremap " ""<left>
"inoremap ' ''<left>
"inoremap ( ()<left>
"inoremap [ []<left>
"inoremap { {}<left>
"inoremap {<CR> {<CR>}<ESC>O
"inoremap {;<CR> {<CR>};<ESC>O

" Syntax for markdown enabled
au BufNewFile,BufFilePre,BufRead *.md set filetype=markdown
au BufNewFile,BufFilePre,BufRead *.txt set filetype=markdown
set syntax=markdown

"
" PLUGINS: https://github.com/junegunn/vim-plug
"

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')
" Challenger-deep-theme: https://github.com/junegunn/vim-plug
Plug 'challenger-deep-theme/vim', { 'as': 'challenger-deep' }
" NERDTree
Plug 'preservim/nerdtree'
" vimwiki
Plug 'vimwiki/vimwiki'
" editorconfig
Plug 'editorconfig/editorconfig-vim'
" Initialize plugin system
call plug#end()

"
" THEME:
"
colorscheme challenger_deep

```

Creamos directorio para spell check:

```shell
$ mkdir -p ~/.vim/spell
$ cd ~/.vim/spell
```

Descargaremos los siguientes archivos:

* es.latin1.spl
* es.latin1.sug
* es.utf-8.spl
* es.utf-8.sug

Descargamos los archivos:

```shell
$ wget https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget -k https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.spl
$ wget https://ftp.vim.org/vim/runtime/spell/es.latin1.sug
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.latin1.sug
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.spl
$ wget --no-check-certificate https://ftp.vim.org/vim/runtime/spell/es.utf-8.sug
```

Creamos los archivos para los diccionarios locales:

```shell
$ touch ~/.vim/spell/es.utf-8.add
$ touch ~/.vim/spell/en.utf-8.add
```

Shortcuts:

* **]s** – Siguiente falta ortográfica
* **[s** – Anterior falta ortográfica
* **z=** – Mostrar sugerencias para una palabra incorrecta.
* **zg** – Añadir una palabra al diccionario.
* **zug** – Deshacer la adición de una palabra al diccionario.
* **zw** – Eliminar una palabra del diccionario.

Instalar herramienta para gestión de plugins: plug-vim:

```shell
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Configuración de Plugins:

```shell
"
" PLUGINS: https://github.com/junegunn/vim-plug
"

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')
" Challenger-deep-theme: https://github.com/junegunn/vim-plug
Plug 'challenger-deep-theme/vim', { 'as': 'challenger-deep' }
" NERDTree
Plug 'preservim/nerdtree'
" vimwiki
Plug 'vimwiki/vimwiki'
" Initialize plugin system
call plug#end()

"
" THEME:
"
colorscheme challenger_deep

```

Guardar vim, salir, y volver a entrar, entonces:

```shell
:PlugInstall
```

## Emulador de terminal Terminator

Instalamos el paquete Terminator:

```shell
$ sudo apt install terminator
```

Shortcuts: TODO.

## El shell zsh con oh-my-zsh

Instalamos el shell zsh:

```shell
$ sudo apt install zsh
```

Instalamos oh my zsh:

```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Cambiamos el shell default:

```shell
$ chsh -s $(which zsh)
```

### Configuración de plugins integrados en zsh

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Para habilitar los diferentes plugins cambiamos:

```shell
plugins=(git)
```

por:

```shell
plugins=(cp colored-man-pages colorize pip python git zsh-autosuggestions zsh-syntax-highlighting)
```

Guardamos y actualizamos la configuración:

```shell
$ source .zshrc
```

### Configuración del tema integrado en zsh

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Para cambiar el tema cambiamos:

```shell
ZSH_THEME="robbyrussell"
```

Por:

```shell
ZSH_THEME="gianu"
```

Guardamos y actualizamos la configuración:

```shell
$ source .zshrc
```

Lista de temas: https://github.com/ohmyzsh/ohmyzsh/wiki/themes.

### Configurando tema powerlevel10k

Instalamos el tema powerlevel10k:

```shell
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k```

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Agregamos la lista de sources:

```shell
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Guardamos y actualizamos la configuración:

```shell
$ source .zshrc
```

Seguimos las indicaciones del asistente de configuración de powerlevel10k que aparecerá al actualizar la shell ZSH y en
caso de que no se ejecute automáticamente el asistente de configuración de powerlevel10k, ejecutaremos la orden

```shell
$ p10k configure
```
Seguir las indicaciones del asistente de configuración y seleccionar las opciones según nuestro gusto personal para usar
ZSH con el tema powelevel10k

Proyectos: https://www.zsh.org/
https://ohmyz.sh/
https://github.com/ohmyzsh/ohmyzsh
https://github.com/romkatv/powerlevel10k
