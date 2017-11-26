--- 
wordpress_id: PSD-22
layout: post
title: simple modified time check in bash
---
I recently got burgled and so I decided to setup a very simple little alerting system with a raspberry pi and the telegram bot api. I didn't want to send too many alerts
so this little script `touch`'s a lockfile so that once it has sent through a picture, it'll wait (at least) a minute before sending through another.

Pretty crude, but it works.

```bash
#!/bin/bash

FILE=$1
CHATID="not a real chat id"
KEY="not a real key"
TIME="10"
URL="https://api.telegram.org/bot$KEY/sendDocument"
LOCKFILE="/home/pi/lockfile"

/usr/bin/mpack -s "Your Security Camera has detected Motion!" $FILE my_alert_address@example.com

if [ "$(( $(date +"%s") - $(stat -c "%Y" $LOCKFILE) ))" -gt "60" ]; then
  touch $LOCKFILE
  curl -F chat_id="$CHATID" -F document=@"$FILE" -F caption="ALERT ALERT! Security" $URL
fi
```
