# password-manager-migration
This repo lists the steps I took to move to a password manager

# Steps
Sign up for a password manager (e.g. LastPass or BitWarden)
Install the software on desktop/mobile

(Optional) Install 2FA software, or buy a Yubikey

# Migration
I wanted to move everything I've ever signed up for into a password manager, and to make sure that I wasn't reusing passwords, or using weak passwords. This involved getting an export from my email providers (GMail and Hotmail), parsing the e-mail domains, and checking each one individually. I had around 1,400 'unique' domains from one provider alone, so it does unfortunately take some time. There are tools to help make this faster though.

# Exporting the data (into mbox format)
GMail:
https://support.google.com/accounts/answer/3024190?hl=en

Hotmail:
https://support.microsoft.com/en-us/office/export-mailbox-and-delete-search-history-in-outlook-com-be5335ea-5f86-41ba-892e-de9e4cb16b87

# Convert from mbox to a list of domains (csv)
https://github.com/YIDING-CODER/MBOX---Email-extractor

# Tidy up the domains to remove subdomains
When going through my lists, I found that a great many of them were duplicates based on the subdomain (e.g. e.example.com, email.example.com), so after much searching I found the following tool which intelligently removes all sub domains, and just returns the main domain. Domain formats are very insopnsistent, so trying to filter it yourself using the number of dots (.) is a bad idea.
https://github.com/john-kurkowski/tldextract
```
for domain in $(cat domainlist); do tldextract $domain | cut -d " " -f 2,3 | sed 's/\ /\./g' >> newdomainlist; done
cat newdomainlist | tr '[:upper:]' '[:lower:]' | sort | uniq > uniquedomainlist
```
Note: There will still be some messyness (e.g. domains that somehow have brackets in them), but this will tidy things up significantly
