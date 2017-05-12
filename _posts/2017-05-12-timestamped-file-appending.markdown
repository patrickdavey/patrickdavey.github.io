--- 
wordpress_id: PSD-18
layout: post
title: Timestamped file appending
---

I had a memory bug I was trying to track down, one of the things I wanted to be able to do was to inspect the memory use of passenger. This tiny little snippet stores the current time into a variable
and then uses that in the rest of the command to append to the same file.

Worked for me :)

```bash

PATRICKTEMP="$(date).txt" ; while true; do sudo passenger-memory-stats | grep dirty >> "$PATRICKTEMP"; sleep 2; done
```
