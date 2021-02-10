---
layout: post
title: Simple Alma printing
tags:
- alma
- printing
- library
- Ex_Libris
---

Quite some time before implementing Alma we had decided that we would no
longer be generating printed notices. The only thing we would be printing
would be hold shelf slips to be placed into requested items before placing
on the hold shelf. This simplifies the task, but I think that the solution
we came up with is extensible if more printing than this is required.

<!--more-->

With Aleph the hold shelf slips were produced using a USB Epson
receipt printer attached to an Aleph workstation. This was convenient
and quick. We decided to just use a standard networked laser printer,
printing A5 slips, rather than ordering a networked receipt printer. We
could always get a receipt printer if necessary. Two years on we are
still using the laser printer and A5 slips.

Within Alma a patron account 'Alma Print' with primary identifier
'alma_print' is defined. This account has the email address
'alma_print@lancaster.ac.uk'.  The 'Alma Print' patron definition is
required so that 'XML To Letter Admin' in 'General Configuration' can
be configured to send the *data* for notice generation. This is a useful
convenience when developing the formatting of letters.

The printer (which happens, in our case, to be 'Printer no. 4')used
for hold shelf slips is defined to have the email address
'alma_print@lancaster.ac.uk'. This is configured in 'Fulfillment
Configuration/Printers'. Then 'Printer no. 4' is defined to service the
default circulation desks in 'Fulfillment Configuration/Printers'.

Now when a hold shelf slip is generated the html formatted email is
sent to the email address 'alma_print@lancaster.ac.uk'. This is picked
up at the university mail hubs and forwarded to the email address
'alma_print@library.lancs.ac.uk'. 'library.lancs.ac.uk' is a Linux server
used by the library to perform a number of tasks, hold shelf slip printing
being one.

In this server there is a Linux user account, 'alma_print'. This is a
standard user account. The server 'library.lancs.ac.uk' is able to do
network printing to the printer 'exit_desk', which is where we want the
hold shelf slips printed.

The widely available and venerable mail processing tool 'procmail'
is installed on this machine. If a configuration file '.procmailrc'
exists for a Linux user account 'procmail' automatically processes all
email received for that user. The '.procmailrc' for 'alma_print' is
configured to store a backup of the email and pass it to a shell script,
'process_mail' which does the necessary printing.

'process_mail' is very simple. A case statement takes action based on
the subject parameter. In our case we are only interested in hold shelf
slips which have a normalized subject of 'Resource_Request_Slip'. A shell
script, 'get_html_from_message', is called which extracts just the html from
the file. 'get_html_from_message' is a tiny 'awk' script. We are not
interested in the attached logo or barcode for our hold slips. The
html is passed to 'html2ps' to convert to postscript and directed to
the defined printer. 'html2ps' is a widely available Linux utility.

It couldn't be any simpler. It has been in production use for 2 years
with no issues. The only management involved is an occasional cleanup of
the logs and backed up emails. Do note however, that due to the amount
of cloud, internet and local infrastructure plumbing between Alma and
the printer that printing of slips is not immediate as was the case
with Aleph. The slip can arrive at the printer seconds after the item
has been scanned. Sometimes it takes much longer. Email infrastructure
is not designed for interactive work. This has entailed small changes
to the local work flow.

Should printing of a wider variety of notices be needed 'process_mail' can
be extended by adding functions and modifying the case statement to act
appropriately based on the subject of the emails received.

The source is available at https://bitbucket.org/ditlul/alma-printing

This post is available at https://developers.exlibrisgroup.com/blog
and http://jhkrug.github.io
