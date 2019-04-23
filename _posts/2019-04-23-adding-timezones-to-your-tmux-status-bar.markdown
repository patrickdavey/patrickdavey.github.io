---
wordpress_id: PSD-24
layout: post
title: Adding timezones to your tmux status bar
---

I quite like having various times displayed in my tmux status bar. I've got family in a few different
countries, and my client is in San Fransico. It look a little fiddling, but, it works ;). The tricksy bit is that
these tmux scripts don't run in the context of your full (bash/zsh) profile, but in a stripped down `.sh` profile, so,
you're a bit more limited in what you can easily run.

There are _quite likely_ nicer ways to do this, but, firstly make a little file with the timezone you're after,
for example, here's one for New Zealand


### Contents of nz.sh
```bash
#!/bin/sh
NZ=`TZ="Pacific/Auckland" date +"%H:%M"`
echo "NZ: $NZ"
```

Then add a path to the script and appropriate colours to your bar...

### status line in .tmux.conf
```bash
set -g status-right-length 80
set -g status-right '#[fg=colour233,bg=colour245,bold] #(~/pathto/nz.sh) #[fg=colour233,bg=colour241,bold] %d/%b #[fg=colour233,bg=colour245,bold] %H:%M'
```

End result looks like:

![tmux times](/images/tmux-times.png)


If you want more, have a look [in my dotfiles repo](https://github.com/patrickdavey/dotfiles)
