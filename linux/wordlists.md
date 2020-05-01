# Intro

This file contains details about useful Linux tools for creating wordlists as well as links to some useful wordlists

# Useful Wordlists
1. <ftp://ftp.openwall.com/pub/wordlists/>
1. <http://www.openwall.com/mirrors/>
1. <https://github.com/danielmiessler/SecLists>
1. <http://www.outpost9.com/files/WordLists.html>
1. <http://www.vulnerabilityassessment.co.uk/passwords.htm>
1. <http://packetstormsecurity.org/Crackers/wordlists/>
1. <http://www.ai.uga.edu/ftplib/natural-language/moby/>
1. <http://www.cotse.com/tools/wordlists1.htm>
1. <http://www.cotse.com/tools/wordlists2.htm>
1. <http://wordlist.sourceforge.net/>

### Generating a simple wordlist
This generates a very simple list using the characters a, b, c, 1 and 2 between 6 and 8 characters long

NOTE: Use man crunch to get the detailed usage information

Change 6 to the minimum length required

Change 8 to the maximum length required

Change abc12 to the characters required (case sensitive)

Change the output file (-o) as required or omit for the output to be sent to stdout

```crunch 6 8 abc12 -o test.txt```

### Generate a simple wordlist using a format
This generates a simple list using the characters a, b, c, 1 and 2 that is 6 characters long and uses the format a@@@@b (starts with a and ends with b and is filled in using all combinations of characters supplied)

Change 6 to the length required (needs to match the format string)

Change abc12 to the characters required (case sensitive)

Change the output file (-o) as required or omit for the output to be sent to stdout

Change the format/pattern (-t) as required

```crunch 6 6 abc12 -o test.txt -t a@@@@b```