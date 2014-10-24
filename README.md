phpcomplete-extended
====================

phpcomplete-extended is a fast, extensible, context aware autocomplete plugin
for PHP composer projects. Initially it reads autoload classmap of a composer
project, parses doc-comments of each class and creates index from them. After
that it auto updates index as you type thanks to
[vimproc.vim](https://github.com/Shougo/vimproc.vim) plugin. Besides
autocomplete this plugin have several code inspection features,

* Includes full core PHP documentation
* See documentation of current word, be it class name, method or property. It is
  context aware.
* Go to definition of a symbol. Also context aware.
* Automatically add use statement of current completed word. Also added plugin
  command of this action.
* If [unite.vim](https://github.com/Shougo/unite.vim/) plugin installed following sources are available,
    * `phpcomplete/files`           : Lists PHP files of the project.
    * `phpcomplete/vendors`         : Lists vendor directories
    * `phpcomplete/extends`         : Lists classes that extends the class guessed from
      the current cursor word.
    * `phpcomplete/implements`      : Lists classes that implements the class guessed
      from the current cursor word.

Demo videos (click on the image to goto youtube)
-----------------------------------------------
## Autocomplete demo video:

[![ScreenShot](http://img.youtube.com/vi/yZYFKslqkC8/maxresdefault.jpg)](http://www.youtube.com/watch?v=yZYFKslqkC8)

## Unite sources demo video:

[![ScreenShot](http://i1.ytimg.com/vi/Wd5G7QA3OFw/maxresdefault.jpg)](http://www.youtube.com/watch?v=Wd5G7QA3OFw)

Installation
-------------

The plugin depends on following plugins,

*  [vimproc.vim](https://github.com/Shougo/vimproc.vim) : Asynchronous library for vim. Required for auto-update index. C
  compiler is needed to build the plugiin. For windows install cygwin or
  msys to get the compiler.
*  [unite.vim](https://github.com/Shougo/unite.vim/): Optional, but enables some good features.

Plugin managers are recommended for installing these plugins. Installation instructions for
various plugin managers are given bellow.

## Pathogen

Issue following commands.

```sh
git clone https://github.com/Shougo/vimproc.vim.git ~/.vim/bundle/vimproc.vim
cd ~/.vim/bundle/vimproc.vim
make
cd ..
git clone https://github.com/Shougo/unite.vim.git ~/.vim/bundle/unit.vim
git clone https://github.com/m2mdas/phpcomplete-extended.git ~/.vim/bundle/phpcomplete-extended
```

## NeoBundle (preferred)
Put these lines in `.vimrc` and issue `source %` command

```vim
NeoBundle 'Shougo/vimproc', {
      \ 'build' : {
      \     'windows' : 'make -f make_mingw32.mak',
      \     'cygwin' : 'make -f make_cygwin.mak',
      \     'mac' : 'make -f make_mac.mak',
      \     'unix' : 'make -f make_unix.mak',
      \    },
     \ }
NeoBundle 'Shougo/unite.vim'
NeoBundle 'm2mdas/phpcomplete-extended'
"your other plugins
NeoBundleCheck
```

If C compiler found for respective environment, `neobundle` will automatically
compile the library.

## Vundle

Put following lines in `.vimrc` and issue `:PluginInstall` command.
```vim
Plugin 'Shougo/vimproc'
Plugin 'Shougo/unite.vim'
Plugin 'm2mdas/phpcomplete-extended'
"your other plugins
"....
```

After installation goto `vimproc` folder and issue `make` command from command
line. C compiler must be installed.

Walk through of the plugin
-------------------------

To enable omni complete add following line to `.vimrc`,

    autocmd  FileType  php setlocal omnifunc=phpcomplete_extended#CompletePHP

Then issuing `<C-X><C-O>` in insert mode will open the completion pop-up menu.

This plugin is tested in Windows and Linux. It should work in OSX also. 

For now this plugin supports [Composer](http://getcomposer.org/) PHP projects.
Upon detecting a composer project the plugin scans classmap generated by `php
composer.phar dumpautoload --optimize` command. By default composer command is
set to `php composer.phar`. The command can be changed by setting
`g:phpcomplete_index_composer_command` global variable. 

The plugin is dependent on `@var`, `@param`, `@return` doc-comments to give proper context aware
autocomplete. So documenting your class will help tremendously. It does not
analyse full class. Just parses the variable declaration to get the relevant
tokens. Does not provide autocomplete suggestion if there is error in code.

Indexing may take some time depending on project size. To decrease index creation time I
would recommend disabling xdebug extension in cli. To do that go to the location
where `php.ini` resides and copy the `php.ini` file as `php-cli.ini` and comment
out the xdebug line. To verify issue `php -m | grep xdebug` in commandline.

For configuration reference see helpdoc.


Auto complete plugin supports
-----------------------------

## [neocomplete](https://github.com/Shougo/neocomplete.vim)

Neocomplete support is built-in and my preferred autocomplete plugin. It needs
vim with lua bindings. To know more about installation refer to the plugin
repository.

## [supertab](https://github.com/ervandew/supertab)

Minimal configuration for supertab support,

    autocmd  FileType  php setlocal omnifunc=phpcomplete_extended#CompletePHP
    let g:SuperTabDefaultCompletionType = "<c-x><c-o>"

## [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)

If `omnifunc` set the omni completer of `YouCompleteMe` should get the completion
candidates. I haven't tested it though.

Extensions
---------

Phpcomplete Extended pluign exposes api hooks so that it can be possible to
provide framework specific autocomplete suggestion. For example facades in
laravel, DIC services in Symfony2 etc. The plugin api divided in two part, `PHP`
and `vim`. PHP part is responsible for creating index related to the framework, 
and vim part is responsible for providing autocomplete menu entries based on the
index. For reference see
[phpcomplete-extended-symfony](https://github.com/m2mdas/phpcomplete-extended-symfony)
and
[phpcomplete-extended-laravlel](https://github.com/m2mdas/phpcomplete-extended-laravel) plugins.

License
-------
MIT licensed.

Acknowledgement 
--------------- 

This plugin would not be possible without the works of 
[Shougo](https://github.com/Shougo),
[shawncplus](https://github.com/shawncplus/),
[tpope](https://github.com/tpope/), [vim-jp](https://github.com/vim-jp),
[thinca](https://github.com/thinca) and many others.

