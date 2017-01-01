---
layout: post
title: Show current folder as iTerm title
category: miscellaneous
---

I feel it is great that iTerm can show current directory name as its title. I did some research and found out this:

```bash
# put it to your ~/.bash_profile
export PROMPT_COMMAND='echo -ne "\033];${PWD##*/}\007"'
```

[Detailed explanation](https://gist.github.com/phette23/5270658):

> Piece-by-Piece Explanation:
> the if condition makes sure we only screw with $PROMPT_COMMAND if we're in an iTerm environment
>
> iTerm happens to give each session a unique $ITERM_SESSION_ID we can use, $ITERM_PROFILE is an option too
>
> the $PROMPT_COMMAND environment variable is executed every time a command is run
> see: ss64.com/bash/syntax-prompt.html
>
> we want to update the iTerm tab title to reflect the current directory (not full path, which is too long)
>
> echo -ne "\033;foo\007" sets the current tab title to "foo"
>
> see: stackoverflow.com/questions/8823103/how-does-this-script-for-naming-iterm-tabs-work
>
> the two flags, -n = no trailing newline & -e = interpret backslashed characters, e.g. \033 is ESC, \007 is BEL
>
> see: ss64.com/bash/echo.html for echo documentation
>
> we set the title to ${PWD##*/} which is just the current dir, not full path
>
> see: stackoverflow.com/questions/1371261/get-current-directory-name-without-full-path-in-bash-script
> 

BTW, I am using `iTerm2`.
