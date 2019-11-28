# VIM Setup for PHP Development

It will be based in [Vim Bootstrap](https://vim-bootstrap.com/) as a starting point since it provider a plugin manager and a basic configuration.

We'll use [Phpactor](https://phpactor.github.io/phpactor/vim-plugin.html)

Some pointer from https://levelup.gitconnected.com/vim-for-react-and-typescript-oh-my-40b700c32cc


## Install VIM

```bash
brew install vim
```

## Starter setup

```bash
curl -s 'http://vim-bootstrap.com/generate.vim' --data 'langs=javascript&langs=html&langs=go&langs=python&editor=vim' > ~/.vimrc
```

Take into account that even tough we're the PHP language is available, we weill use different plugins



## Phpactor


Add this to `.vimrc.local.bundles`

```vim
Plug 'phpactor/phpactor', {'for': 'php', 'do': 'composer install'}
```

And this to `.vimrc.local`

```vim
" Include use statement
nmap <Leader>u :call phpactor#UseAdd()<CR>

" Invoke the context menu
nmap <Leader>mm :call phpactor#ContextMenu()<CR>

" Invoke the navigation menu
nmap <Leader>nn :call phpactor#Navigate()<CR>

" Goto definition of class or class member under the cursor
nmap <Leader>o :call phpactor#GotoDefinition()<CR>

" Show brief information about the symbol under the cursor
nmap <Leader>K :call phpactor#Hover()<CR>

" Transform the classes in the current file
nmap <Leader>tt :call phpactor#Transform()<CR>

" Generate a new class (replacing the current file)
nmap <Leader>cc :call phpactor#ClassNew()<CR>

" Extract expression (normal mode)
nmap <silent><Leader>ee :call phpactor#ExtractExpression(v:false)<CR>

" Extract expression from selection
vmap <silent><Leader>ee :<C-U>call phpactor#ExtractExpression(v:true)<CR>

" Extract method from selection
vmap <silent><Leader>em :<C-U>call phpactor#ExtractMethod()<CR>
``
