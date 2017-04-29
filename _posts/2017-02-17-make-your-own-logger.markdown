--- 
wordpress_id: PSD-15
layout: post
title: Make your own logger
---
Sometimes it's handy to have your own logger, so that you can log out just the bits that you're interested in. Thanks to [Jeanine from littleforestconsulting.com](http://www.littleforestconsulting.com/) for this neat snippet

```ruby
# Set up a custom logger.  View output in /log/my_error.log
class MyLogger < Logger
  def format_message(severity, timestamp, progname, msg)
    formatted_time = timestamp.strftime("%Y-%m-%d %H:%M:%S.") << timestamp.usec.to_s[0..2].rjust(3)
    "[%s] %s\n" % [formatted_time, msg]
  end
end

logfile = File.open(Rails.root.to_s + '/log/my_error.log','a')
logfile.sync = true
MYLOG = MyLogger.new(logfile)
# TO USE: MYLOG.info(<stuff>) (or whatever level of debug info you want to use)
```
