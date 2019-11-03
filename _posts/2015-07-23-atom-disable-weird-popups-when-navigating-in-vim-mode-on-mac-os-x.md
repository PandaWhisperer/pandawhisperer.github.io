---
layout: post
date: 2015-07-23 01:09:13 GMT
tags:
- atom
- editor
- mac os x
- coding
- programming
- bug
- fix
- solution
title: "Atom: Disable weird popups when navigating in Vim-Mode on Mac OS X"
---
I've recently decided to give GitHub's [Atom Text Editor](https://atom.io) another chance, after the long await 1.0 release. Of course, as a long time Vim user, the first package I installed was [vim-mode](https://atom.io/packages/vim-mode), along with [ex-mode](https://atom.io/packages/ex-mode) to make me feel right at home. So far, so good.

Most Vim commands worked perfectly, even advanced motion commands like `ciw` or `daW` are working like they're supposed to. However, one thing that bothered me was a weird popup that would appear every so often when navigating using the `l` key, as demonstrated on the [GitHub issue][iss] that was filed regarding this problem:

![](/images/posts/47dd0df22b66b59e8c7a5e2cada1eb876d9d8cb5fa7283effc7e247500a92b09.gif)

<!-- more -->

Thanks to GitHub user [@rastasheep][rs], however, there appears to be a [solution][sol]:

> Mac OS X Lion introduced a new, iOS-like context menu when you press and hold a key that enables you to choose a character from a menu of options. If you are on Lion try it by pressing and holding down `e` in any app that uses the default NSTextField for input.
>
> It's a nice feature and continues the blending of Mac OS X and iOS features. However,it's a nightmare to deal with in Atom if you're running vim mode, as it means you cannot press and hold `h/j/k/l` to move through your file. You have to repeatedly press the keys to navigate.
>
>     defaults write com.github.atom ApplePressAndHoldEnabled -bool false
>
> Alternately, if you want this feature disabled globally, you can enter this:
>
>     defaults write -g ApplePressAndHoldEnabled -bool false
>
> In either case you'll need to restart Atom for the change to take place.
>
> Happy coding!

[iss]: https://github.com/atom/vim-mode/issues/175
[rs]: https://github.com/rastasheep/
[sol]: https://gist.github.com/rastasheep/bfc8266eeb58b899054c
