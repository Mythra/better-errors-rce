# BetterErrors - RCE #

This repository provides an example of how to perform a DNS Rebinding Attacks
when `binding_of_caller` is active to provide a REPL. This is filed as an issue
on BetterErrors: [here](https://github.com/charliesome/better_errors/issues/350).
Unfortunately it's still not fixed.

## Activating the Attack From Scratch ##

_***NOTE***: I've setup a web server to print out /etc/passwd, but if you want to reproduce this locally cause you don't trust me than you can._

Setup Phase:

* Create a Webserver with one port to run your webserver on open to the world ( I recommend 3000, but to each their own, it doesn't really matter as long as you can open the same local port on your machine ).
* Make sure the webserver has a public IP. We'll need this in order to use a free rebinder provided by a [google security researcher](http://twitter.com/taviso).
* Once you have your public IP grab a rebind address (source code for rebind is here: https://github.com/taviso/rbndr, and a tool to setup your own rebind domain is here: https://lock.cmpxchg8b.com/rebinder.html . You'll want to alternate between 127.0.0.1, and whatever the public IP of your webserver is.
* Using the HTML File (the exploit file), replace: `22e98617.7f000001.rbndr.us:3000` With your rebinder domain, and port, and setup a url that will trigger a better errors page if a standard 404 doesn't.
* Install python2 on your webserver that contains the exploit html file.
* Run: `python -m SimpleHTTPServer <myport>`
* Follow the steps listed for those who want to use my test domain (just navigating to your domain instead, and using your special port).

## Steps to Reproduce From Test Domain ##

* Get a fully working local setup of your tool.
* And finally startup a rails server on port 3000.
* Once you've started up a local server go to: `http://22e98617.7f000001.rbndr.us:3000` (***NOTE***: /accounts/999999999999999999 generates an error page). ***NOTE***: You may see your local rails app. Just keep refreshing (on firefox you have to close your window, and restart :( ) until you get the exploit page.
* Click: `Attempt Exploit`
* Wait for the exploit to work. ***NOTE***: Depending on local settings this can take multiple minutes to work. If chrome says we're slowing your browser down just keep waiting.
* Notice your /etc/passwd contents in the console.
