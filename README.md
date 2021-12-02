# ~/.vim folder

Setting up Vim on Linux
```bash
$ git clone https://github.com/vim/vim.git
$ cd vim/src/
$ make
$ sudo make install
```
If issues occur, it might be that some dependencies are missing. Try:
```bash
$ sudo apt install make build-essential libncurses5-dev
```

> **Mastering Vim Build a software development environment with Vim and Neovim (p. 105-6)**

Vim 8 introduced a native way to load plugins, by expecting the files to be in a directory
tree under .vim/pack . Vim 8 expects the following structure of the files:
- `.vim/pack/plugins/opt/` is used for plugins you want to manually load
- `.vim/pack/plugins/start/` is used for plugins you always load

Within this folder a further folder **start** is needed to hold plugins. Vim will pick up any 
packages added to this folder and automatically load the plugins.

Optionally another folder **opt** may be created to hold packages that are not loaded automatically. 
Packages added in the opt folder may be loaded using:

```bash
:packadd packagename
```

In addition, you'll want to add the following two lines to your .vimrc file to load the
documentation for all the plugins:
```bash
packloadall           " Load all plugins."
silent! helptags ALL  " Load help for all plugins."
```

You can manage your plugins yourself (with some overhead) by using Git submodules to
download the plugins and keep them up to date.

Initialize a Git repository in the .vim directory (a one-time step):
```bash
$ cd ~/.vim
$ cp ~/.vimrc vimrc
$ git init
$ git add .
$ git commit -m "Initial commit."
$ git remote add origin https://github.com/henry-the-vietnamese/vim.git
$ git push -u origin master
```
---
---
---
Clone .vim:
```bash
git clone --recursive git://github.com/henry-the-vietnamese/vim.git ~/.vim
```

Replicating the repository on a machine:
Clone the repository (recursively to clone plugins as well):
```bash
git clone --recursive https://github.com/username/reponame.git
```

Symlink .vim and .vimrc:
```bash
ln -sf reponame ~/.vim
ln -sf reponame/vimrc ~/.vimrc
```

Generate helptags for plugins:
```bash
vim
:helptags ALL
```

To enable submodules:
```bash
cd ~/.vim
git submodule init
```

Add a plugin as a submodule:
```bash
$ git submodule add https://github.com/scrooloose/nerdtree.git pack/plugins/start/nerdtree
$ git add .gitmodules vim/pack/plugins/start/nerdtree
$ git commit -m "Added NERDTree plugin."
```

Now, every time you want to update your plugins, you can run the following:
To update foo:
```bash
$ cd ~/.vim/pack/plugins/start/foo
$ git pull origin master
```
It is recommended to first ```git fetch``` origin master a plugin, review changes, and then ```git merge```.
To update all the plugins:
```bash
$ cd ~/.vim
$ git submodule foreach git pull origin master
```

To delete a plugin, remove the submodule with the following steps:
```bash
$ cd ~/.vim/pack/plugins/start/
$ git submodule deinit rails.vim/
$ git rm -r rails.vim
$ rm -r .git/rails.vim/
" Now the rails.vim/ folder is gone or may be left with rails.vim/doc/tags"
$ cd doc/tags
$ rm tags
$ cd ..
$ rmdir doc/
$ cd ..
$ rmdir rails.vim/
" Now the complete rails.vim plugin is deleted."
$ git commit -m "Delete rails.vim plugin."
$ git push
```

On another machine, if a ```git pull``` for the main repository leads to uncommitted changes in the submodules (as a few plugins got updated), perform ```git submodule``` update to change the recorded state of the submodules.
