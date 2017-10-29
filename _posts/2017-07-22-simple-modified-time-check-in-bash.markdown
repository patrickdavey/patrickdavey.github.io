--- 
wordpress_id: PSD-22
layout: post
title: simple modified time check in bash
---

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
  curl -F chat_id="$CHATID" -F document=@"$FILE" -F caption="ALERT ALERT! Security" $URL
  touch $LOCKFILE
fi
```
