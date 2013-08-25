# A set of useful git hooks

These hooks automate some tasks with the help of git.

## Installation

    git clone git@github.com:greg0ire/git_template.git ~/.git_template

## Configuration

Set the newly cloned repo as your git template directory. This will tell git to
populate new repositories created with either `git clone` or `git init` with
the content of this directory. By default, it uses `/usr/share/git-core/templates`.

    git config --global init.templatedir '~/.git_template'

## What's inside ?

For the moment, mostly hooks related to tools from the php ecosystem.

### Exuberant Ctags hook

Updates `.git/tags` file by scanning the project with the ctags command. It is
configured for a php project. To make vim look for this file in the `.git`
directory, you can install Tim Pope's [fugitive][4] or simply add
`set tags+=.git/tags` to your .vimrc - some plugins (like [ctrlp-tjump][5])
require this to see the tags even if fugitive is installed.

### Exuberant Ctags hook (patched ctags version)

Same as above but uses a [patched version of ctags][6] with improved PHP language
support. Only use one of the ctags hooks at a time.

### Composer hook

This set of scripts monitor `composer.lock` changes and updates your vendor
dependencies when appropriate. Additionally, it checks composer.json for
validity on pre-commit. It assumes Composer is [globally installed][1].

### Sismo hook

The post-commit hook makes [Sismo][2] run each time you commit. It is a post-commit
hook because Sismo is a *local* Continuous Testing Server, which means you can
build before you push.

ou must configure the path to the sismo executable, and you may do so globally,
like this:

    git config --global --add hooks.php-sismo.path /usr/share/nginx/sismo.php

You must also configure the slug of your project:

    git config --add hooks.php-sismo.slug my-slug

Don't forget to enable the plugin if you want to use it.

### Doctrine hook

This hooks runs the `doctrine:schema:validate` task of a Symfony project and
updates / migrates your database depending on the presence of a
`doctrine-migrations` folder in your vendor directory.

### Junk checker hook

Checks for user defined phrases that you don't want to commit to your
repository, such as `var_dump()`, `console.log()` etc.

This can be overridden by doing a:

    git commit --no-verify

This hook is language-agnostic.

## Usage

By default, no hook will run. You must configure the hooks you need:

    git config hooks.enabled-plugins php/composer
    git config --add hooks.enabled-plugins php/ctags
    git config --add hooks.enabled-plugins junkchecker

If you want to enable a plugin on every project, use the `--global` option:

    git config --global --add hooks.enabled-plugins some_plugin

## Contributing

Creating your php plugin is as easy as creating a php folder under `hooks/php`.
You can then create hooks in it. For the moment, only the following are supported
(because I'm lazy)

* post-commit
* post-checkout
* post-merge
* post-rewrite

If you need to add configuration variables git configuration, you should prefix
them with the path to your hook, which is obviously unique. This will avoid
collisions.

## Running the test suite

Get this random [shunit2][7] fork in ~/src :

    git clone git@github.com:deniscostadsc/shunit2.git

and run `tests/all.sh`.

## Credits

Inspired by [Tim Pope][3]

[1]: https://github.com/composer/composer#global-installation-of-composer-manual
[2]: http://sismo.sensiolabs.org/ "A local Continuous Testing Server"
[3]: http://tbaggery.com/2011/08/08/effortless-ctags-with-git.html
[4]: https://github.com/tpope/vim-fugitive
[5]: https://github.com/ivalkeen/vim-ctrlp-tjump
[6]: https://github.com/shawncplus/phpcomplete.vim/wiki/Patched-ctags
[7]: https://code.google.com/p/shunit2/
