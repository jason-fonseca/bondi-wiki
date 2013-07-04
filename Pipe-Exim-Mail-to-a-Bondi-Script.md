1, sudo apt-get install exim4
1. dpkg-reconfigure exim4-config
1. Choose smarthost, define the current host with the domain name (like ezimerchantdev.com)
1. Don't bother relaying messages for anyone.
1. Finish setup, exit 
1. Edit /etc/exim4/exim4.conf.template

You should begin by creating a domain list such as
`domainlist cmd_domains = ezimerchantdev.com`

Add the below to the router section
(put it below `begin router`)
<code><pre># command router
cmd_router:
  driver = accept
  domains = +cmd_domains
  transport = cmd_transport</code></pre>

put it below `begin transports`
Add this to the transports section
<code><pre>cmd_transport:
  debug_print = "T: using cmd_transport"
  driver = pipe
  command = /tmp/test.pl
  delivery_date_add
  user = nwo
  envelope_to_add</code></pre>

There are lots of options as to whats send to the script, you will likely want to review them. Two such options above are 'delivery_date_add' and 'envelope_to_add', which add the date of delivery and the envelope 'to' field to the data send to the script. 

The 'command' option is where you list you command. You can add exim config variables to that command string and when exim runs the command it will place those values in the args of the command. 

As for the script, here is a sample. The message data is pipe'd to the script via STDIN. This script is not very usefull, but demonstrates how to read STDIN in perl.
<code><pre>#!/bin/sh
env CONTENT_TYPE=message/rfc822 BONDI_APPROOT=/home/nwo/approot bondi mail.esp</code></pre>

And thats it! Now its up to you to do something with the email. You can use some other language than perl or even compile a program, just read STDIN and examine the program arguments

Extract from here: http://rantage.com.au/item/Send-email-via-a-pipe-to-a-script-with-exim/