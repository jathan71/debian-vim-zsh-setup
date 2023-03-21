# Debian GNU/Linux, Vim y ZSH

## Introducción

En esta guía se describe la configuración del sistema operativo GNU/Linux para adaptarlo a un ambiente de trabajo para
realizar tareas relacionadas a la administración de:

* GNU/Linux personal
* Servidores GNU/Linux
* Recursos de cómputo en la nube
* Clusters Kubernetes
* Desarrollo de software

Se utilizará Software Libre, es decir, software GNU, el kernel Linux y en específico la distro Debian GNU/Linux 11
Bullseye en una arquitectura x86 de 64-bit.

Para esto necesitamos dejar algunas cosas bien configuradas:

* Entornos de desarrollo
* Gestores de paquetes
* Editor de textos
* Emulador de terminal
* Entorno de shell
* Cliente VPN
* Navegador web

## Objetivos

Realizaremos las siguientes actividades orientadas a instalar y configurar:

* Herramientas de linea de comando GNU
* Gestor de paquetes Apt
* Editor de textos vim
* Gestor de ventanas i3
* El shell zsh con oh-my-zsh
* Herramientas de búsqueda en línea de comandos
* Conjunto de herramientas de línea de comandos
* Herramientas de gestión de cómputo en la nube

## Gestor de paquetes APT

El programa `apt` es la herramienta de gestión de paquetes en `Debian`.

Apt ya viene instalado en Debian 11 Bullseye, por lo que no hay que instalarlo manualmente.

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

Instalaremos los programas `wget` y `curl`, los cuales usaremos para descargar cosas desde
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

Atajos de teclado:

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

## Gestor de ventanas i3

i3 es un gestor de ventanas de mosaico ligero, que tiene como principales características abrir las ventanas de un
programa distribuyéndolas de manera proporcional y ajustada en toda el área de un escritorio, así como utilizar atajos
de teclado similares a Vim para abrir, navegar y cerrar ventanas.

https://i3wm.org/
https://i3wm.org/docs/userguide.html

Instalamos los siguientes paquetes:

```shell
$ sudo apt install i3 lxterminal xscreensaver lightdm xfce4-power-manager
```

Creamos o editamos el archivo /home/usuario/.i3/config y adaptamos el contenido a nuestras preferencias personales:

```shell
# This file has been auto-generated by i3-config-wizard(1).
# It will not be overwritten, so edit it as you like.
#
# Should you change your keyboard layout somewhen, delete
# this file and re-run i3-config-wizard(1).
#

# i3 config file (v4)
#
# Please see http://i3wm.org/docs/userguide.html for a complete reference!

set $mod Mod4

# Font for window titles. Will also be used by the bar unless a different font
# is used in the bar {} block below. ISO 10646 = Unicode
font -misc-fixed-medium-r-normal--13-120-75-75-C-70-iso10646-1
# The font above is very space-efficient, that is, it looks good, sharp and
# clear in small sizes. However, if you need a lot of unicode glyphs or
# right-to-left text rendering, you should instead use pango for rendering and
# chose a FreeType font, such as:
# font pango:DejaVu Sans Mono 10

# Use Mouse+$mod to drag floating windows to their wanted position
floating_modifier $mod

# start a terminal
bindsym $mod+Return exec lxterminal

# aplications shorcuts
bindsym Shift+$mod+f exec pcmanfm
bindsym Shift+$mod+w exec google-chrome
bindsym Shift+$mod+v exec vlc
bindsym Control+$mod+d exec thunderbird
bindsym Shift+$mod+p exec pidgin
bindsym Shift+$mod+t exec mousepad
bindsym Control+$mod+l exec libreoffice
bindsym Control+$mod+s exec soundconverter
bindsym Control+$mod+e exec easytag
bindsym Control+$mod+t exec tuxguitar
bindsym Shift+$mod+a exec audacious
bindsym Control+$mod+c exec chromium
bindsym Shift+$mod+x exec hexchat
bindsym Shift+$mod+b exec /home/jathan/Documents/Scripts/bc
bindsym Shift+$mod+s exec /home/jathan/Documents/Scripts/sc
#bindsym Shift+$mod+h exec /home/jathan/Documents/Scripts/hb

# kill focused window
bindsym $mod+Shift+q kill

# start dmenu (a program launcher)
bindsym $mod+d exec dmenu_run
# There also is the (new) i3-dmenu-desktop which only displays applications
# shipping a .desktop file. It is a wrapper around dmenu, so you need that
# installed.
# bindsym $mod+d exec --no-startup-id i3-dmenu-desktop

# change focus
bindsym $mod+j focus left
bindsym $mod+k focus down
bindsym $mod+l focus up
bindsym $mod+ntilde focus right

# alternatively, you can use the cursor keys:
bindsym $mod+Left focus left
bindsym $mod+Down focus down
bindsym $mod+Up focus up
bindsym $mod+Right focus right

# change focus
bindsym Mod1+Tab focus left
bindsym Mod1+Shift+Tab focus right

# move focused window
bindsym $mod+Shift+j move left
bindsym $mod+Shift+k move down
bindsym $mod+Shift+l move up
bindsym $mod+Shift+ntilde move right

# alternatively, you can use the cursor keys:
bindsym $mod+Shift+Left move left
bindsym $mod+Shift+Down move down
bindsym $mod+Shift+Up move up
bindsym $mod+Shift+Right move right

# split in horizontal orientation
bindsym $mod+h split h

# split in vertical orientation
bindsym $mod+v split v
# enter fullscreen mode for the focused container
bindsym $mod+f fullscreen

# change container layout (stacked, tabbed, toggle split)
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split

# toggle tiling / floating
bindsym $mod+Shift+space floating toggle

# change focus between tiling / floating windows
bindsym $mod+space focus mode_toggle

# focus the parent container
bindsym $mod+a focus parent

# focus the child container
#bindsym $mod+d focus child

# change to previous/next workspace
bindsym Control+Mod1+Left workspace 1
bindsym Control+Mod1+Right workspace 2
bindsym Control+Mod1+Down workspace 3
bindsym Control+Mod1+Up workspace 4

# switch to workspace
bindsym $mod+1 workspace 1
bindsym $mod+2 workspace 2
bindsym $mod+3 workspace 3
bindsym $mod+4 workspace 4
bindsym $mod+5 workspace 5
bindsym $mod+6 workspace 6
bindsym $mod+7 workspace 7
bindsym $mod+8 workspace 8
bindsym $mod+9 workspace 9
bindsym $mod+0 workspace 10

# move focused container to workspace
bindsym $mod+Shift+1 move container to workspace 1
bindsym $mod+Shift+2 move container to workspace 2
bindsym $mod+Shift+3 move container to workspace 3
bindsym $mod+Shift+4 move container to workspace 4
bindsym $mod+Shift+5 move container to workspace 5
bindsym $mod+Shift+6 move container to workspace 6
bindsym $mod+Shift+7 move container to workspace 7
bindsym $mod+Shift+8 move container to workspace 8
bindsym $mod+Shift+9 move container to workspace 9
bindsym $mod+Shift+0 move container to workspace 10

# reload the configuration file
bindsym $mod+Shift+c reload
# restart i3 inplace (preserves your layout/session, can be used to upgrade i3)
bindsym $mod+Shift+r restart
# exit i3 (logs you out of your X session)
bindsym $mod+Shift+e exec "i3-nagbar -t warning -m 'You pressed the exit shortcut. Do you really want to exit i3? This will end your X session.' -b 'Yes, exit i3' 'i3-msg exit'"

# resize window (you can also use the mouse for that)
mode "resize" {
        # These bindings trigger as soon as you enter the resize mode

        # Pressing left will shrink the window’s width.
        # Pressing right will grow the window’s width.
        # Pressing up will shrink the window’s height.
        # Pressing down will grow the window’s height.
        bindsym j resize shrink width 10 px or 10 ppt
        bindsym k resize grow height 10 px or 10 ppt
        bindsym l resize shrink height 10 px or 10 ppt
        bindsym ntilde resize grow width 10 px or 10 ppt

        # same bindings, but for the arrow keys
        bindsym Left resize shrink width 10 px or 10 ppt
        bindsym Down resize grow height 10 px or 10 ppt
        bindsym Up resize shrink height 10 px or 10 ppt
        bindsym Right resize grow width 10 px or 10 ppt

        # back to normal: Enter or Escape
        bindsym Return mode "default"
        bindsym Escape mode "default"
}

bindsym $mod+r mode "resize"

# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
bar {
        position top status_command i3status
        }

# autostart applications
exec --no-startup-id sudo /home/jathan/Documents/Scripts/inicio-brillo
exec --no-startup-id feh --bg-fill /home/jathan/Pictures/Debianr1.png
#exec --no-startup-id feh --bg-fill /home/jathan/Pictures/Japan/Japan_Night3.jpg
#exec --no-startup-id feh --bg-center /home/jathan/Pictures/Aachen/Aachen_schoner_Sonnenuntergang1.jpg
#exec --no-startup-id feh --bg-max /home/jathan/Pictures/Dragon_Ball/Bardock/Bardock1.jpg
#exec --no-startup-id feh --bg-fill /home/jathan/Pictures/praia_do_leblon.jpg
exec --no-startup-id xscreensaver -no-splash
exec --no-startup-id xfce4-power-manager
#exec --no-startup-id /home/jathan/Documents/Scripts/xinput-wheelfunction
exec --no-startup-id /home/jathan/Documents/Scripts/inicio-conky
#exec --no-startup-id /home/jathan/Documents/Scripts/inicio-policykit
#exec --no-startup-id /home/jathan/Documents/Scripts/inicio-chromium
#exec --no-startup-id /home/jathan/Documents/Scripts/inicio-liferea
```

Atajos de teclado:
Tecla Super= tecla entre crtl y alt izquierdos

Super+1: Escritorio 1
Super+2: Escritorio 2
Super+3: Escritorio 3
Super+4: Escritorio 4
Super+Enter: lxterminal
Super+Shift+f: pcmanfm
Super+Shift+w: navegador web
Super+f: Cambiar ventana a pantalla completa
Super+j: Ir a ventana izquierda
Super+k: Ir a ventana inferior
Super+l: Ir a ventana superior
Super+ñ: Ir a ventana derecha
Super+v: Indicar el inicio de la siguiente ventana verticalmente debajo de la actual
Super+h: Indicar el inicio de la siguiente ventana horizontalmente a la derecha de la actual

Se puede personalizar la estética con temas de íconos y colores basados en GTK3 utilizando lxappearance

https://www.gnome-look.org/browse?cat=135

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
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Editamos el archivo de configuración de zsh:

```shell
$ vim .zshrc
```

Cambiamos el tema de gianu por powerlevel10k:

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
ZSH con el tema powelevel10k.

Proyectos: https://www.zsh.org/
https://ohmyz.sh/
https://github.com/ohmyzsh/ohmyzsh
https://github.com/romkatv/powerlevel10k
