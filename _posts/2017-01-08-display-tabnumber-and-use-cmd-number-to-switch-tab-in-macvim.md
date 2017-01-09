---
layout: post
title: Display tab number and use cmd + number to switch tabs in MacVim
category: miscellaneous
---

It is great that we can use `cmd` + number to navigate tabs in a MacVim window
just like iTerm and Chrome.

Put the following code into your `~/.vimrc` and do the magic for you:

```
if has("gui_macvim")

  " Switch to specific tab numbers with Command-number
  noremap <D-1> :tabn 1<CR>
  noremap <D-2> :tabn 2<CR>
  noremap <D-3> :tabn 3<CR>
  noremap <D-4> :tabn 4<CR>
  noremap <D-5> :tabn 5<CR>
  noremap <D-6> :tabn 6<CR>
  noremap <D-7> :tabn 7<CR>
  noremap <D-8> :tabn 8<CR>
  noremap <D-9> :tabn 9<CR>

  " Command-0 goes to the last tab
  noremap <D-0> :tablast<CR>
endif

```

And of course, you can use `cmd` + `shift` + `[` to the previous tab and `cmd` + `shift` + `]` to the next tab.

By default, MacVim does not display the tab index in the tab title. It is
great that MacVim can display the index so that you can easily use `cmd`
+ number to navigate. You can put the following line into your `~/.vimrc`:


```
if has('gui_running')
  set guitablabel=⌘%N@%M%t
endif
```

But somehow, it does not work for me. I have to use a key to toggle the
setting:


```
function! ShowTabNumber()
  if has('gui_running')
    set guitablabel=⌘%N@%M%t
  endif
endfunction
map <F2> :call ShowTabNumber()<CR>
```
