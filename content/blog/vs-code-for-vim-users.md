+++
author = "Ryan King"
categories = ["vim", "visual studio code"]
date = "2019-01-24T18:00:00"
description = "A Vim user's guide to setting up Visual Studio Code"
featured = "vs-code.jpg"
featuredalt = "using Visual Studio Code"
featuredpath = "date"
linktitle = "Visual Studio Code for Vim Users"
title = "Visual Studio Code for Vim Users"
type = "post"

+++

# An Editor Tale as Old as January 1, 1970

Like many people, my journey to Vim did not start at Vim; I spent many days and many nights downloading editors,
installing plugins, and learning key bindings that have sinse been forgotten. I had been using SublimeText for a few
years when I went into college, but like many poor soles, my first course was a Java course, which broke me, and,
as despserate as I have ever been, I booked an appointment at the sinister medical practice of Dr. Java.

With the horrors of java.lang.ClassNotFoundException behind me, I realized that I was still here, that in fact the
sun does rise, and most of all, I was ready to love again. With both a desire in my heart and strong refusal to write
multi-file projects in the Python IDLE, I opened up SublimeText, and told it with all the certainity that I could muster
that I was perfectly fine using the unregistered version and I would happily spend my $50 elsewhere. After a wild
Friday Night of once again editing my configuration and installing plugins, I was off to the races with a nice color
scheme, good bindings, and an integrated interpreter.

Time passed, and like many, I became unpleased with how long it had been since I had gone through and overhauled all my
configurations, so I decided to try my hand with an editor older than me that some claim ages like a fine cheese: vim.
After trying and failing to learn it with the built in docs, I discovered the the wonderful game/tutorial,
[Vim Adventures](https://vim-adventures.com/), and after a few hours of solving the surprisingly tricky puzzles using
vim keys, I was navigating vim much like a master classical pianist dances his fingers across the black and white keys.

Leaves fell, snow came down in sheets then melted, summer heat sweltered, but one thing stayed consistent: the tapping
away at my keyboard while cob webs grew on my mouse. I had an affair with the Emacs distribution
[Spacemacs](http://spacemacs.org/) that turned Emacs into a more-or-less IDE with vim navigation, had a summer love
with [neovim](https://neovim.io/), but found myself time and time again falling back into the arms of my one true love,
vim, or so I thought.

When I left the world of academia and course website written on the most basic HTML, it was recommended by many of my
now coworkers that I give the editor, Visual Studio Code a try. Hearing tales of the slow load times of its electron
brother Atom and having familiarity with the horrors of Visual Studio, I scoffed, but I tried it, and it was okay. I
installed a Vim key plugin, which worked well, but I found navigating the whole interface was not nearly as fluid as
moving between tmux panes, so, to the disdain of most around me, I was back to my love, vim, once more.

After a few months of tap, tap, tapping away at my keyboard, soemtimes noticing things like autocompletion for
unimported libraries out of the corner of my eye, I decided that there's no way, as a junior software engineer that
I am right, and everyone around me is wrong, they must know something that I don't. So I decided to give Visual Studio
Code another try, a good, honest, try. I would install some plugins, setup some vindings, really dive deep into the,
settings, and see if I could make it something that, like Vim, I didn't think about when I was using it. I never thought
about how to get in or out of the terminal, I never thought about switching to a different file, I never thought about
moving between splits, I just used. And it worked.

# Why I Really Made the Switch

As much as I enjoy weird functional languages, 99.999% of my current development is in Go, it's what I write everyday
for work, so my syntax is always fresh and I know a lot of common libraries of the top of my head, and most of what I
wnat do, even in my free time, can be done very well in Go. The VS Code Go plugin is awesome, has all of the features
that caused me to fall in love with [vim-go](https://vimawesome.com/plugin/vim-go-sparks-fly) many years ago and then
some. In addition, the auto complete is second to none, so if I'm using a library I'm unfamiliar with and I have a
struct, I just type the name of my object and a dot, and VS Code shows me the many options I have along with the notes
and parameters in full detail, it's one of the features that worked so well for me that I even missed it after my short
initial stint with the editor. I have tried a lot of libraries for autocomplete in vim/neovim and none have been able
to replicate the functionality, plus having it just work out of the box was... nice. It's also very nice to beable to
quickly just click and run individual tests or test files.

# Installing Visual Studio Code

The editor can be downloaded from the [official website](https://code.visualstudio.com/) and installed, or installed
on macOS as a homebrew cask:

~~~bash
> brew cask install visual-studio-code
~~~

It can be installed on Arch Linux from the
[Arch User Repository](https://aur.archlinux.org/packages/visual-studio-code-bin/) using the AUR helper of your
choosing. I like `yay` because it is written in Go and I like Go, plus it seems to be the best helper right now, but
there's a new "best one" weekly it seems.

~~~bash
> yay -S visual-studio-code-bin
~~~

I am sure it is available in other distros' various package managers, but I do not know because I am one of
_those people_ who only use Arch because I have to compensate for my short comings with the complexity of my OS.

After it is installed, I would recommend immediately installing the shell command by typing `⌘⇧P` typing
out "install," arrowing down to "Shell Command: Install 'code' binary to PATH," which will install the `code` command
to your PATH surprisingly enough, so you will be able to now type commands such as:

~~~bash
> code projects/my-stupid-project-name
~~~

To open up the target directory or file in a new Visual Studio Code window. Neat.

# A Few Notes

* Most of my work these days is done on macOS, so whenever I say `⌘-something`, on a Windows or Linux machine it most likely `ctrl-something`.
* If I say to enter a command into VS Code, it is meant to be entered into your "command palette" with `⌘⇧P`.

# Enter: VSCodeVim

{{< img-fit "12u" "vs-code-vim.png" "the VSCodeVim GitHub page" "date" >}}

Surprisingly enough the first thing we are going to do is install the VS Code Vim plugin. I have personally only used
[VSCodeVim](https://github.com/VSCodeVim/Vim) because it suits all my needs, has a lot of the functionality from popular
vim plugins such as [surround](https://vimawesome.com/plugin/surround-vim) and
[commentary](https://vimawesome.com/plugin/commentary-vim) (let us all take a moment to give thanks to Tim Pope), as
well as many other wonderful plugins; the full list is on the GitHub page, which you should read because it is full of
valueable information, which probably makes most of this post unnecessary, so don't read it, I guess. It is also by far
the most popular Vim plugin on the marketplace.

To install it, first we need to clone a package manager into some random dot folder in your `$HOME` directory. Only
kidding. Click the weird box shape on the left that looks sort of like the Manjaro Linux logo or press `⌘⇧K`, then type
"Vim" and it should be the first plugin that appears. Click install, reload, and you should be off to the races.

{{< img-fit "12u" "vs-code-vim-ext.png" "the VSCodeVim plugin page within VS Code" "date" >}}

# Configuring Visual Studio Code

You should now be able to navigate files with vim keys like you would in vim, but we can make it better. Let's start off
with some basic VS Code settings that I have found make my experience a whole let better. Start off by opening your
`settings.json` by opening your command palette and letting the "Preferences: Open Settings (JSON)" command rip. If you
would like, there is a GUI, but this is faster and easier to show with minimal screenshots or long winded explanations.
Here are some setitngs that I'm a big fan of.

~~~json
"editor.rulers": [120]
~~~

Moving from an 80 character line limit to a 120 character line limit was a big positive change in my life, almost as
big as getting my driver's license or turning 21. It is just so freeing not to have to make ugly line break to follow
some rule that you don't even know why you follow.

~~~json
"editor.minimap.enabled": false
~~~

I do not know what psychopath decided that using 10-20% of your screen realestate to show you a zoomed out view of your
file was a good idea. Looking at you SublimeText.

~~~json
"editor.fontSize": 10
~~~

This change was big for me, I think VS Code font is scaled up or my terminal font is scaled down. Either way, during my
first use, VS Code felt super cramped since I usually work with at least two splits. If the default 12pt font size
looks good and does not hurt your eyes, keep it, it's your life.

~~~json
"terminal.external.osxExec": "iTerm.App"
~~~

The handy command `⌘⇧c` opens an external terminal window, so if you use something like iTerm, it'll use that. Even
better, if you have an existing iTerm window, it'll just add a tab instead of a whole new window. Super handy if you
want to do something like check logs for something unrelated to your current project without flooding the integrated
terinal with unrelated history.

~~~json
"terminal.integrated.shell.osx": "/usr/local/bin/zsh"
~~~

I use zshell.

~~~json
"workbench.editor.showTabs": false
~~~

Since I'm a vim guy, I don't really care for tabs, I only care what's open in the current split, so it's nice being able
to see the entire filename and path within the project all the time. See an issue with this setting in the caveats
section below.

~~~json
"go.autocompleteUnimportedPackages": true,
~~~

Enables the great Go autocomplete I raved about earlier.

~~~json
"python.linting.pylintPath": "/Users/rking/.pyenv/shims/pylint",
~~~

I unfortunately have to edit some Python every now and again, and this makes sure the Python plugin uses the linter
for my currently active virtualenv so that way it knows about all the packages and such.

~~~json
"python.linting.pylintArgs": ["--max-line-length=120"],
~~~

120 > 80 both mathematically and as a line length.

This last section is done in a different JSON config, specifically keybindings.json. It can be accessed similarly by
using the "Preferences: Open Keyboard Shortcuts (JSON)" command.

~~~JSON
[
    {
        "key": "ctrl+h",
        "command": "workbench.action.focusLeftGroup"
    },
    {
        "key": "ctrl+j",
        "command": "workbench.action.focusAboveGroup"
    },
    {
        "key": "ctrl+k",
        "command": "workbench.action.focusBelowGroup"
    },
    {
        "key": "ctrl+l",
        "command": "workbench.action.focusRightGroup"
    },
    {
        "key": "ctrl+shift+h",
        "command": "workbench.action.moveActiveEditorGroupLeft"
    },
    {
        "key": "ctrl+shift+j",
        "command": "workbench.action.moveActiveEditorGroupDown"
    },
    {
        "key": "ctrl+shift+j",
        "command": "workbench.action.moveActiveEditorGroupUp"
    },
    {
        "key": "ctrl+shift+j",
        "command": "workbench.action.moveActiveEditorGroupRight"
    },
    {
        "key": "ctrl+w s",
        "command": "workbench.action.splitEditorDown"
    },
    {
        "key": "ctrl+w v",
        "command": "workbench.action.splitEditorRight"
    },
    {
        "key": "cmd+m",
        "command": "workbench.action.terminal.toggleTerminal"
    }
]
~~~

The first four commands let me use `C-{hjkl}` to navigate between splits (known as groups in VS Code terminology) like
I do in vim/tmux. The second four let me move editor groups around to change the order. The next two let me create
vertical/horizontal splits using `C-w {sv}` like I do in vim. They don't open _exactly_ the same since the second time
you make a vertical split, it sets it to thirds instead of a half and two
quarters, but I have yet to find that a hinderance, if anything, it's almost better because when you close the third
vertical split, it goes back to halves. The last one is my preferred terminal hotkey.

# Configuring VSCodeVim

Most of this is going to be a repeat stuff you can read in the project's docs, but I'll just give an example and one
rule to follow: never ever set vim key bindings in the main "keybindings" options for VS Code, you will find weird
behavior. Doing it this way will ensure that the bindings only impact your vim setup. Note that we're back in the
settings JSON, not the keybindings JSON.

I find the syntax kind of strange, but here is an example of some of my vim key bindings that I missed in VS Code that
I moved over relatively easily:

~~~json
"vim.normalModeKeyBindings": [
    {
        "before": ["g", "h"],
        "commands": [
            "cursorHome",
        ]
    },
    {
        "before": ["g", "j"],
        "commands": [
            "cursorBottom",
        ]
    },
    {
        "before": ["g", "k"],
        "commands": [
            "cursorTop",
        ]
    },
    {
        "before": ["g", "l"],
        "commands": [
            "cursorEnd",
        ]
    },
],
~~~

What those do is make `g-{hjkl}` go to the beginning of the line, bottom line, top line, or end of the line
respectively, which I find a lot more coherent than `^`, `$`, `gg`, `G`. You can rebind in all the vim major modes:
"vim.normalModeKeyBindings", "vim.insertModeKeyBindings", etc., and all follow the same syntax. "before" is the keys
you press _before_ the command happens, and you either put in the command(s) you want to happen in a list, or you can
use the "after" setting to specify an existing keybinding to emulate. So

~~~json
{
    "before": ["g", "h"],
    "after": ["g", "j"]
}
~~~

Would cause the effect of `gj` whenever I pressed `gh`.

I haven't been able to find a list of all the keybinding commands like "workbench.focusLeft", but if you open up the
keymaps GUI by going using the "Preferences: Keymaps" command or hitting `⌘K ⌘S`, you can search for commands, and if
one is already bound, you can copy the actual command by right clicking.

# VSCode Tips and Trick

* One plugin I really missed from vim was [Tagbar](https://vimawesome.com/plugin/tagbar), but I found the objects menu accessed with `⌘⇧o` to fill the void quite nicely.
* If you want to edit some random file, don't bother looking for an extension, VS Code will alert you once it opens if it has an extension to help.
* Some other hotkeys I regularly use:
  * `⌘P`: Equivalent of [ctrlp](https://vimawesome.com/plugin/ctrlp-vim-red), fuzzy file matcher, builtin, works like a charm.
  * `⌘B`: Toggles the left bar.
  * `⌘⇧E`: Takes me to the file tree.
  * `⌘⇧M`: Takes me to a pane with all the linting issues in my entire project.
  * `⌘⇧D`: Takes me to the Docker side menu, more on that in the plugins section.
  * `⌘⇧K`: Takes me to the Kubernetes side menu, again, more on that in the plugins section.

# Useful Plugins

Here are some additional plugins that I have installed that I find useful and have my leaving the editor less and less:

* [Go](https://github.com/Microsoft/vscode-go): I don't know how you'd write Go without it.
* [Python](https://github.com/Microsoft/vscode-python): Everyone writes Python.
* [Lisp](https://github.com/mattn/vscode-lisp): Everyone should write Common Lisp.
* [ATS](https://github.com/ldeleris/ats-vscode): I like ATS in case you haven't noticed.
* [GitLens](https://github.com/eamodio/vscode-gitlens): Really useful inline Git information and a nice sidemenu to view information quickly.
* [Docker](https://github.com/microsoft/vscode-docker): Outside of Dockerfile syntax highlighting, which is alone worth the install, adds another handy dandy side menu to see all your containers, images, and registeries.
* [Kubernetes](https://github.com/Azure/vscode-kubernetes-tools): Helm chart syntax highlighting, plus another really nice sidebar for easily looking at cluster information.
* [YAML](https://github.com/redhat-developer/vscode-yaml): In case the above plugin wasn't a clue, I edit a lot of YAML, this plugin adds syntax highlighting and makes navigating a large YAML file with the object brwoser easy.
* [Better TOML](https://github.com/bungcip/better-toml): I don't use rust, but I do edit TOML files sometimes.
* [markdownlint](https://github.com/DavidAnson/vscode-markdownlint): Who knew markdown was so strict?
* [Markdown Preview Mermaid Support](https://github.com/mjbvz/vscode-markdown-mermaid): If you happen to write Mermaid diagrams, makes your life a lot easier.
* [LaTeX Workshop](https://github.com/James-Yu/LaTeX-Workshop): One of the best LaTeX writing experiences I've ever had.
* [Settings Sync](https://github.com/shanalikhan/code-settings-sync): If you edit on multiple computers, like I do, makes keeping your configs synced an after thought.
* [Bracket Pair Colorizer](https://github.com/CoenraadS/BracketPair): I got used to this functionality during my Spacemacs stint, not _as_ useful for Go, but required for not going crazy editing Lisp.

# Eye Candy (Not Useful) Plugins

Here are some plugins that aren't useful, but I like a pretty setup:

* [Gruvbox Themes](https://marketplace.visualstudio.com/items?itemName=tomphilbin.gruvbox-themes) I use gruvbox dark medium on everything, I find it really easy on the eyes with really good contrast.
* [Material Icon Theme](https://github.com/PKief/vscode-material-icon-theme): Some pretty icons because you gotta have those.
* [Titlebar-Less VSCode for mac](https://github.com/lehni/vscode-titlebar-less-macos): A stupid plugin to make your stupid editor look better in windowed mode on your stupid mac.
* [Fix VSCode Checksums](https://github.com/lehni/vscode-fix-checksums): Makes the corruption errors go away if you're stupid enough to install the above plugin.

# Caveats

Two things that really stick out to me and still become a pain every now and again. The first being that the file
browser and terminal not treated like "groups" so I cannot simply `ctrl-{hjkl}` to them like I would into a
NERDTree, but I rarely find that a hinderance since the cmd+p file switcher works so well. The other is that I cannot
remap `:q` to close the entire group instead of the current editor, so if I have a bunch of tabs open and want to
completely remove a vertical split, it takes a few more key strokes. I have hopes that the second of the two complaints
will be corrected as the plugin continues to mature.

# Closing Thoughts

I still use vim, probably everyday, but for what I feel it was born to do, edit files. Whenever I need to make a quick
change or edit a config file, I still find myself typing those three magic letters and going to town, but when working
on a large project with test suites and spralling dependencies, I use something that was made to do that, Visual Studio
Code. Who's to say that that the open source go language server will not get to the point that I can go back to using
vim full time, or another shinny new editor will catch my eye, but for now, I am happy, I am writing code, and
thinking about the code, not the writing of the code.
