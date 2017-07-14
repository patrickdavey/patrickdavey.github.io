--- 
wordpress_id: PSD-21
layout: post
title: using a debugger on the exercism ECMAScript trail
---

I like [exercism](http://exercism.io). It's a really nice site for learning a new language. Anyway, one thing I've wanted to get a bit better at is ECMAScript 2015 (ES6/Harmony) the newest version of JavaScript.

When I get stuck, I really like to be able to drop into a debugging environment, have a poke around, see what variables are doing, what the state of the world is. I didn't find it very straightforward, and some Googl'ing came in useful. Eventually I found the incantation to do what I needed, thanks [to this GitHub comment](https://github.com/facebook/jest/issues/1652#issuecomment-275887373)

1. `npm install jest-environment-node-debug --save-dev`
2. `node --debug-brk --inspect ./node_modules/.bin/jest -i --env jest-environment-node-debug` (node 7) or
3. `node --inspect-brk --inspect ./node_modules/.bin/jest -i --env jest-environment-node-debug` (node 8)
4. Then go to [chrome://inspect](chrome://inspect) (node 8) to open up the debugger and press play until you drop into your debugger.

Well, it worked for me anyway ;)
