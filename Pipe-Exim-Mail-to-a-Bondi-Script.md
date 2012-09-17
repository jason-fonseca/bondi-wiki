end email via a pipe to a script with exim 
Posted 10:33:11 PM on Wed, 17th August 2011 by Dean 
Here is a short example of how to have exim4 pipe emails to a script. Basically it uses the 'pipe' transport to feed the email to the script via STDIN. I'm going to assume the reader is familiar with exim enough that i won't explain what transports and routers are, or where they belong in the exim config file. In this example, i will have exim feed all emails for a single domain to the script, you can configure exim to use other criteria such as user names, regular expressions, database lookups etc. The exim4 documenation will explain these options. Anyway.

You should begin by creating a domain list such as
domainlist cmd_domains = cmd.your-domain.com
Then either add cmd_domains to local_domains, or look for the dnslookup driver and add the domain next to the list next to local_domains. 

Add the below to the router section
<code><pre># command router
cmd_router:
  driver = accept
  domains = +cmd_domains
  transport = cmd_transport</code></pre>

Add this to the transports section
<code><pre>cmd_transport:
  debug_print = "T: using cmd_transport"
  driver = pipe
  command = /tmp/test.pl
  delivery_date_add
  envelope_to_add</code></pre>

There are lots of options as to whats send to the script, you will likely want to review them. Two such options above are 'delivery_date_add' and 'envelope_to_add', which add the date of delivery and the envelope 'to' field to the data send to the script. 

The 'command' option is where you list you command. You can add exim config variables to that command string and when exim runs the command it will place those values in the args of the command. 

As for the script, here is a sample. The message data is pipe'd to the script via STDIN. This script is not very usefull, but demonstrates how to read STDIN in perl.
<code><pre>#!/bin/sh
env CONTENT_TYPE=message/rfc822 BONDI_APPROOT=/home/nwo/approot bondi mail.esp</code></pre>

And thats it! Now its up to you to do something with the email. You can use some other language than perl or even compile a program, just read STDIN and examine the program arguments

Extract from here: http://rantage.com.au/item/Send-email-via-a-pipe-to-a-script-with-exim/