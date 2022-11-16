# Mathematica

Mathematica was installed using the `installer.sh` file found through mathematicas website. All settings were used as default and it created folders in:

### Instillation Directory: `/usr/local/Wolfram/Mathematica/12.3`

Directories (Possibly not initially created):
`/home/andrew/.Mathematica
/usr/share/Mathematica`

## Mathematica Packages

### xAct

The exact package was added by putting the package in the folder:
`/home/andrew/.Mathematica/Applications/xAct/`

# Pass

`pass` is a system used for storing passwords.

The folder `~/.password-store` was copied from main computer using git. In the future, this git repository could be done using the raspberrypi (needs to change its password soon I think).

The secret and private key were temporarily created from the main device and copied over by a physical process:

```bash
sudo gpg --export ${ID} > public.key
sudo gpg --export-secret-key ${ID} > secret.key
```

After copying/updating the files the secret key had to be imported. Commands run are:

```bash
sudo apt install pass gnupg2
```

The secret key and private key were imported from the physical device by:

```bash
sudo gpg --import public.key
sudo gpg --import secret.key
```

This should all be encompassed in the directory:

`~/.password-store`

# Python

## Python 3.6

Installed `python3` using:

```bash
sudo apt update
sudo apt install python3.6
```

### Pip (python3)

```bash
sudo apt update
sudo apt install python3.venv
sudo apt install python3-pip
```

## Python 2

Installed `python2` using:

```bash
sudo apt update
sudo apt install python2
```

### Pip (python2)

```bash
sudo apt update
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
sudo python2 get-pip.py
```

## Check Install worked

```
andrew@dirac:~$ python3 -m pip --version
```

Should give

`pip 20.3.4 from /usr/lib/python3/dist-packages/pip (python 3.9)
andrew@dirac:~$ python3 --version
Python 3.9.2`





```bash
python2 -m pip --version
```

Should give

`pip 20.3.4 from /usr/local/lib/python2.7/dist-packages/pip (python 2.7)
python2 --version
Python 2.7.18`

## Python Packages:

Ensure `pip` is upto date:

```bash
python3 -m pip install --upgrade pip setuptools wheel
```

Installed packages through pip:

```bash
python3 -m pip install numpy
python3 -m pip install pandas
python3 -m pip install matplotlib
```

or

```bash
python2 -m pip install pandas
```

## Python Style Guides

we install pylint for checking goodness of code:

```bash
sudo apt install pylint
```

We also install `autopep8` on both versions of python

```bash
python3 -m pip install --upgrade autopep8
python2 -m pip install --upgrade autopep8
```

Pylint is ran by giving it a filename of a python script:

```bash
pylint <my_code.py>
```

And we can run autopep8 to modify in line with:

```bash
autopep8 --in-place --aggressive --aggressive <my_code.py>
```

### Matplotlib tkinter issue

For some reason you can't install `_tkinter` package through `pip`. To install use the following commands to `python2` and `python3` respectively

```bash
sudo apt install python-tk
sudo apt install python3-tk
```

# Latex

## Tex Live

```bash
sudo apt install texlive-full
```

## Tex Live Manager

By this installation process we should now have the tex live manager

```bash
man tlmgr
```

## Tex Live

```bash
sudo tlmgr init-usertree
```

https://tug.org/texlive/acquire.html

and followed the link to "Installing TeX Live over the internet. I downloaded the tarball for unix systems. Un tar'd with

```bash
tar -xvzf install-tl-unx.tar.gz
```

We now change to the resulting unpacked directory. Now, supposedly we can just run the file `install-tl`

Let's try this:

```bash
sudo ./install-tl
```

Seems to run fine now

## Tex Live Bashrc requirements

The installation completed fine. It ended with notes to add ccertain files to paths:

Add `/usr/local/texlive/2021/texmf-dist/doc/man` to `$MANPATH`.
Add `/usr/local/texlive/2021/texmf-dist/doc/info` to `$INFOPATH`.
Most importantly, add `/usr/local/texlive/2021/bin/x86_64-linux`

We add these files inside of the ~/.bashrc file.

## Tex live packages

Finally for the last time after running we found that the folder tlpkg in `/usr/local/texlive/2021` does not have permissions

so after change with:

```bash
chmod 777 tlpkg
```

`tlmgr install physics` works properly. Yay

## Latexmk (Use alternative)

The `latexmk` command is very powerfull as it allows for automatically updating pdf files.

Simply run

```bash
latexmk --pdf --pvc document.tex < /dev/null
```

and the pdf file will continuously update.

If vim-plug, and vimtex are already installed then we can use that by default 

```vim
g:vimtex_compiler_latexmk={continuous:1}
```

which should imply that the updates are smooth and continuous.

## Vimtex alternative

With `vimtex` setup, inside of of the latex file simply run:

`\ll`

This will begin the compiling process and will keep it constantly up to date everytime you save.

# Vimtex

Given that we already have vim installed, we also want to install the package `vimtex` which gives us very usefull macros to speed up our typing speed.

## Vim plugin manager

First we need to install a vim plugin manager which will let us install vimtex and manage it for us.

## Vim-plug

We will continue with `vim-plug`:

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

This created the file ` ~/.vim/autoload/plug.vim`

If documentation is needed in the future this file is very detailed.

Next in the `~/.vimrc` file we add the lines:

```textile
call plug#begin()
Plug 'lervag/vimtex'
call plug#end()
```

## Vim-plug updated to Vundle

Vim plug seems a little old and not used as much as Vundle so I am going to use Vundle going forward it works very similar. I followed steps from https://github.com/VundleVim/Vundle.vim and is installed via:

```
 git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

We then use the following boilerplate at the top of our `.vimrc` file:

```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

I then used this formatting going forward.


## Vim-snipmate

Snipmate allows us to use snippets of code that we expect to have to run many times, think of for loops or equations, tables in LaTeX.  We add this by following instructions: https://github.com/garbas/vim-snipmate. This adds to our .vimrc file:
```md
  Plugin 'MarcWeber/vim-addon-mw-utils'
  Plugin 'tomtom/tlib_vim'
  Plugin 'garbas/vim-snipmate'

  " Optional:
  Plugin 'honza/vim-snippets'
```

with the exception that we use `Plug` and not `Plugin`.

However, we still get issues with finding the snippets to reference. For this purpose we add the package `Pathogen`

## Vim-fugitive

Allows for git commands to be run inside of vim. For hotkeys see:
https://github.com/tpope/vim-fugitive

## Vim-Command+t

Press command + t to find a file navigation tool inside of vim

## Reloading

Now we reload Plugs with

```vim
:PlugInstall
```

Inside of vim

# Syncthing

To install syncthing the steps of this following webpage we completed:

https://pimylifeup.com/raspberry-pi-syncthing/

# Tablet Writing

For the moment going to try and use `Write`. I followed the install instructions from this stackexchange post:

https://askubuntu.com/questions/698080/note-taking-app-for-to-use-with-pen-tablets

# DnD Latex Template

## Texmfhome

To use this package like a template we need to create a folder called `temf`. Create:

```bash
mkdir "$(kpsewhich -var-value TEXMFHOME)/tex/latex/"
```

Then download from github and extract file:

```bash
git clone https://github.com/rpgtex/DND-5e-LaTeX-Template.git "$(kpsewhich -var-value TEXMFHOME)/tex/latex/dnd"
```

# Node.js

Following this link:

https://github.com/nodesource/distributions/blob/master/README.md

I ran the following command

```bash
curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash -
sudo apt-get install -y nodejs
```

# Radeon Video card monitoring

I found a package called:

```bash
sudo apt install radeontop
```

which when ran as root gives you details on how well its running

# MarkText

Download from here:

https://github.com/marktext/marktext/blob/develop/docs/LINUX.md

to edit the auto launching of marktext the editing file will be in
`~/.local/share/applications/marktext.desktop`

# Desktop File Creation

To run app images without needing to click on the file make a file similar to:

```textile
[Desktop Entry]
Name=MarkText
Comment=Next generation markdown editor
Exec=marktext %F
Terminal=false
Type=Application
Icon=marktext
Categories=Office;TextEditor;Utility;
MimeType=text/markdown;
Keywords=marktext;
StartupWMClass=marktext
Actions=NewWindow;

[Desktop Action NewWindow]
Name=New Window
Exec=marktext --new-window %F
Icon=marktext
```

with the fie name:

```bash
vim $HOME/.local/share/applications/marktext.desktop
```

then ensure that it was created correctly with:

```bash
desktop-file-validate marktext.dekstop
```

lastly to update the desktop run:

```bash
update-desktop-database $HOME/.local/share/applications/
```

# Plex Video Server

Plex is a video server that essentially is intended to act as a own personal netflix to add things like meta data to personal dvd and mp4s

We follow the steps from the following link:

[Quick-Start &amp; Step by Step Guides for Plex Media Server | Plex Support](https://support.plex.tv/articles/200264746-quick-start-step-by-step-guides/)

Acccess the download from here:

[Media Server Downloads | Plex Media Server for Windows, Mac, Linux, FreeBSD and More](https://www.plex.tv/media-server-downloads/)

And install this with:

```bash
sudo dpkg -i plexmediaserver_1.25.6.5577-c8bd13540_amd64.deb
```

To launch this for the first time we run the following initiation:

```bash
sudo /etc/init.d/plexmediaserver start
```

This command did not appear for me. Perhaps the link is mislabeled. Instead I hit the `windows` key and typed `plex` and the launcher appeared instead. This opened up a web browser that prompted me to setup/login to an account. I created an account with my password manager.

When the server is running, the web app should be available at the following URL:

http://localhost:32400/web/index.html#!/

To add a file to the server either direct it to the `Movies` or `TV` sub folder in the directory `/var/lib/PlexMediaServer`
